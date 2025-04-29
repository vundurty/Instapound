<script>
    import { supabase } from "$lib/supabase";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import Login from "$lib/components/login.svelte";
    import Logout from "$lib/components/logout.svelte";

    let user = {};
    let error = '';

    onMount(async () => {
        const { data: { session }, error: sessionError } = await supabase.auth.getSession();
        if (sessionError) {
            error = sessionError.message;
        } else if (session) {
            user = session.user;
            console.log(user);
            goto('/');
        }
    });
    
</script>

<div class="flex flex-row items-center justify-center h-screen w-full space-x-12 text-gray-800 font-inter">
    {#if Object.keys(user).length === 0}
        <Login />
    {:else} 
        <div class="flex flex-col items-center justify-center p-10">
            <p class="font-logo text-[4rem] mb-10">Instapound</p>
            <p class="text-gray-500 font-semibold mb-10 w-68 text-center">Welcome back, {user.email}</p>
            <!-- <div>Home coming soon</div> -->
            <Logout />
        </div>
    {/if}  
</div>