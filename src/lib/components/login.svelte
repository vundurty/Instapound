<script>
    import { supabase } from "$lib/supabase";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let user = {}; 
    let error = '';
    let email = ''; 
    let password = '';

    // Disable button if inputs are empty
    $: isFormValid = email.trim().length > 0 && password.trim().length > 0;

    onMount(async () => {
        const { data: { session }, error: sessionError } = await supabase.auth.getSession();
        if (sessionError) {
            error = sessionError.message;
        } else if (session) {
            user = session.user;
            goto('/');
        }
    });

    async function signInWithEmail() {
        const { data, error: signInError } = await supabase.auth.signInWithPassword({
            email,
            password,
        });
        if (signInError) {
            error = signInError.message;
        } else if (data.session) {
            user = data.session.user;
            goto('/');
        }
    }
    
</script>

<div class="flex flex-row items-center justify-center h-screen w-full space-x-12 text-gray-800 font-inter">
    <img src="/images/1.png" alt="Hero" width="600" />
    <div class="flex flex-col items-center justify-center p-10">
        <p class="font-logo text-[4rem] mb-10">Instapound</p>
        {#if error}
            <p class="text-sm pb-5 text-rose-500">{error}</p>
        {/if}
        <input bind:value={email} placeholder="Email" type="email" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>
        <input bind:value={password} placeholder="Password" type="password" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350] placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0" />
        <button on:click={signInWithEmail} disabled={!isFormValid} class="disabled:opacity-50 disabled:cursor-not-allowed text-white text-sm p-2 w-68 mt-2 rounded-md bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">Log in</button>
        <div class="flex items-center my-4">
            <hr class="w-28 border-t border-gray-300" />
            <span class="mx-4 text-gray-500 text-sm font-[500]">OR</span>
            <hr class="w-28 border-t border-gray-300" />
        </div>
        <a href="/accounts/password/reset" class="font-[400] text-sm my-10">Forgot password?</a>
        <div class="flex flex-row items-center text-sm gap-1">
            <p>Don't have an account?</p>
            <a href="/accounts/signup" class="text-sky-500 font-[500]">Sign up</a>
        </div>
    </div>    
</div>