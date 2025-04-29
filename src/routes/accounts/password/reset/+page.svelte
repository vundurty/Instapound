<script>
    import { supabase } from "$lib/supabase";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let email = '';
    let loading = false;
    let errorMsg = '';

    $: isFormValid = email.trim().length > 0;

    onMount(async () => {
        const { data, error } = await supabase.auth.getSession();
        if(error) {
            errorMsg = error.message;
        } else if (data.session) {
            goto('/');
        }
    })
    
    async function resetPassword(){
        loading = true;
        const { data, error } = await supabase.auth.resetPasswordForEmail(email, {
            redirectTo: 'http://localhost:5173/accounts/password/reset/confirm',
        });
        if(error) {
            errorMsg = error.message;
        }
        loading = false;
        email = '';
    }; 

</script>


<div class="flex flex-col justify-between items-center h-screen w-full">
    <header class="flex justify-center items-center h-15 w-full bg-white border-b border-gray-300">
        <div class="flex flex-row items-center justify-between w-[58rem]">
            <a href="/" class="font-logo text-4xl">Instapound</a>
            <div>
                <a href="/accounts/login" class="text-white text-sm p-1.5 px-4 w-72 mt-2 rounded-md bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">Log in</a>
                <a href="/accounts/signup" class="text-sky-500 text-sm p-2 w-68 mt-2 hover:cursor-pointer font-[600]">Sign up</a>
            </div>
        </div>
    </header>
    <div class="border border-gray-300 flex flex-col justify-center items-center bg-white rounded-xs">
        <div class=" flex flex-col p-10  justify-center items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-20 mb-5">
                <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 10.5V6.75a4.5 4.5 0 1 0-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 0 0 2.25-2.25v-6.75a2.25 2.25 0 0 0-2.25-2.25H6.75a2.25 2.25 0 0 0-2.25 2.25v6.75a2.25 2.25 0 0 0 2.25 2.25Z" />
            </svg>              
            <p class="text-gray-900 font-semibold mb-2 w-68 text-center">Trouble logging in?</p>
            <p class="text-stone-500 text-sm mb-2 w-80 text-center">Enter your email, phone, or username and we'll send you a link to get back into your account.</p>
            {#if errorMsg}
                <p class="text-sm py-2 text-rose-500 w-80 text-center">{errorMsg}</p>
            {/if}
            <input bind:value={email} type="email" placeholder="Email" class=" border border-stone-300 p-2 py-3 w-74 my-2 placeholder:text-left font-[350] placeholder:font-[400 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-lg outline-0"/>      
            <button on:click={resetPassword} disabled={!isFormValid} class="disabled:cursor-not-allowed disabled:opacity-50 text-white text-sm p-2 w-74 mt-2 rounded-lg bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">
                {#if loading}
                    <svg class="animate-spin w-full" fill="#FFFFFF" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path d="M0 10.996c.484-5.852 5.145-10.512 10.996-10.996v2.009c-4.737.473-8.515 4.25-8.987 8.987h-2.009zm13.004-8.987c4.737.473 8.515 4.25 8.987 8.987h2.009c-.484-5.852-5.145-10.512-10.996-10.996v2.009zm-2.008 19.982c-4.737-.473-8.515-4.25-8.987-8.987h-2.009c.484 5.852 5.145 10.512 10.996 10.996v-2.009zm10.995-8.987c-.473 4.737-4.25 8.514-8.987 8.987v2.009c5.851-.484 10.512-5.144 10.996-10.996h-2.009z"/></svg>
                {:else}
                    Send login Link
                {/if}
            </button>
            <div class="flex flex-col items-center my-10">
                <div class="flex items-center ">
                    <hr class="w-28 border-t border-gray-300" />
                    <span class="mx-4 text-gray-500 text-sm font-[500]">OR</span>
                    <hr class="w-28 border-t border-gray-300" />
                </div>
                <a href="/accounts/signup" class="text-sm font-[500] my-5">Create new account</a>
            </div>
        </div>
        <a href="/accounts/login" class="flex flex-col items-center p-3 text-sm font-[500] bg-stone-100 w-full border border-stone-300 hover:cursor-pointer hover:text-stone-500">
            <p>Back to login</p>
        </a>
    </div>
    <div class="">

    </div>
</div>