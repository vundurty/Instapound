<script>
    import { supabase } from "$lib/supabase";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let password = '';

    $: isFormValid = password.trim().length > 0;

    onMount(async () => {
        const { data, error } = await supabase.auth.getSession();
        if(error) {
            console.error(error.message);
        } else if (data.session) {
            goto('/');
        }
    })

    async function resetPassword(){
        await supabase.auth.resetPasswordForEmail(email, {
            redirectTo: 'http://example.com/account/update-password',
        })
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
            <input bind:value={password} type="password" placeholder="New password" class=" border border-stone-300 p-2 py-3 w-[14rem] my-7 placeholder:text-left font-[350] placeholder:font-[400 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>
            <input bind:value={password} type="password" placeholder="New password, again" class=" border border-stone-300 p-2 py-3 w-[14rem] my-2 placeholder:text-left font-[350] placeholder:font-[400 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>      
            <button on:click={resetPassword} disabled={!isFormValid} class="disabled:cursor-not-allowed disabled:opacity-50 text-white text-sm p-2 w-[14rem] mt-7 rounded-lg bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">Reset password</button>
        </div>
    </div>
    <div class="">

    </div>
</div>