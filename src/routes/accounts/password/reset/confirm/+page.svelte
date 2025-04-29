<script>
    import { supabase } from "$lib/supabase";
    import { goto } from "$app/navigation";

    let password = '';
    let repassord = '';
    let errorMsg = '';
    let loading = false;

    $: isFormValid = password.trim().length > 0;

    async function resetPassword(){
        loading = true;
        
        if( password !== repassord ){
            errorMsg = "Passwords do not match";
            return
        }

        loading = false;
        password = '';
        repassord = '';
        
        const { data, error } = await supabase.auth.updateUser({ password: password });
        if (error) {
            errorMsg = error.message
        } else {
            goto('/')
        }
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
        <div class=" flex flex-col py-14 px-5  justify-center items-center">            
            <p class="text-gray-900 font-semibold mb-2 w-68 text-center text-sm">Create A Strong Password</p>
            <p class="text-stone-500 text-xs mb-2 w-[14rem] text-center">Your password must be at least 6 characters and should include a combination of numbers, letters and special characters (!$@%).</p>
            {#if errorMsg}
                <p class="text-sm py-2 text-rose-500">{errorMsg}</p>
            {/if}
            <input bind:value={password} type="password" placeholder="New password" class=" border border-stone-300 p-2 py-3 w-[14rem] my-7 placeholder:text-left font-[350] placeholder:font-[400 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>
            <input bind:value={repassord} type="password" placeholder="New password, again" class=" border border-stone-300 p-2 py-3 w-[14rem] my-2 placeholder:text-left font-[350] placeholder:font-[400 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>      
            <button on:click={resetPassword} disabled={!isFormValid} class="disabled:cursor-not-allowed disabled:opacity-50 text-white text-sm p-2 w-[14rem] mt-7 rounded-lg bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">
                {#if loading}
                    <svg class="animate-spin w-full" fill="#FFFFFF" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path d="M0 10.996c.484-5.852 5.145-10.512 10.996-10.996v2.009c-4.737.473-8.515 4.25-8.987 8.987h-2.009zm13.004-8.987c4.737.473 8.515 4.25 8.987 8.987h2.009c-.484-5.852-5.145-10.512-10.996-10.996v2.009zm-2.008 19.982c-4.737-.473-8.515-4.25-8.987-8.987h-2.009c.484 5.852 5.145 10.512 10.996 10.996v-2.009zm10.995-8.987c-.473 4.737-4.25 8.514-8.987 8.987v2.009c5.851-.484 10.512-5.144 10.996-10.996h-2.009z"/></svg>
                {:else}
                    Reset password
                {/if}
            </button>
        </div>
    </div>
    <div class="">

    </div>
</div>