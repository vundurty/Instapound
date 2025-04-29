<script>
    import { supabase } from "$lib/supabase";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let errorMsg = '';
    let email = '';
    let password = '';
    let name = '';
    let username = '';

    $: isFromValid = email.trim().length > 0 && password.trim().length > 0 && name.trim().length > 0 && username.trim().length > 0;

    onMount(async () => {
        const { data, error } = await supabase.auth.getSession();
        if(error){
            errorMsg = error.message;
        } else if(data.session){
            goto('/'); 
        }
    });

    async function signUpNewUser() {
        const { data, error } = await supabase.auth.signUp({
            email,
            password,
        })
        if(error){
            errorMsg = error.message;
        } else {
            const { data, error: userError } = await supabase.from('users').insert([
                { email, name, username }
            ]);
            if (userError) {
                errorMsg = userError.message;
            } else {
                goto('/');
            }
        }
    }
    
</script>

<div class="flex flex-col justify-center items-center h-screen w-full">
    <div class="border border-gray-300 flex flex-col justify-center items-center p-10 bg-white rounded-xs">
        <p class="font-logo text-[4rem] mb-5">Instapound</p>
        <p class="text-gray-500 font-semibold mb-10 w-68 text-center">Sign up to see photos and videos from your friends.</p>
        {#if errorMsg}
            <p class="text-sm pb-5 w-68 text-rose-500">{errorMsg}</p>
        {/if}
        <input bind:value={email} type="email" placeholder="Email" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350 placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0"/>
        <input bind:value={password} type="password" placeholder="Password" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350] placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0" />
        <input bind:value={name} type="text" placeholder="Full Name" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350] placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0" />
        <input bind:value={username} type="text" placeholder="Username" class="border border-stone-300 p-2 w-68 placeholder:text-left font-[350] placeholder:font-[350] placeholder:text-stone-500 mb-2 bg-gray-50 text-xs rounded-xs outline-0" />        
        <p class="font-[400] text-xs text-gray-500 w-[16rem] text-center py-2 ">People who use our service may have uploaded your contact information to Instagram. <a href="/accounts/signup" class="text-blue-900">Learn More</a></p>
        <p class="font-[400] text-xs text-gray-500 w-[16rem] text-center py-2 ">By signing up, you agree to our <a href="/accounts/signup" class="text-blue-900">Terms , </a> <a href="/accounts/signup" class="text-blue-900"> Privacy Policy </a> <a href="/accounts/signup" class="text-blue-900"> and Cookies Policy .</a>
        </p>    
        <button disabled={!isFromValid} on:click={signUpNewUser} class="disabled:opacity-50 disabled:cursor-not-allowed text-white text-sm p-2 w-68 mt-2 rounded-md bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">Sign up</button>
    </div>
    <div class="flex flex-col items-center my-4 font-[400] text-sm border border-gray-300 py-5 w-[22rem] bg-white rounded-xs">
        <p>Have an account?</p>
        <a href="/accounts/login" class="text-sky-500 font-[500]">Log in</a>
    </div>
</div>