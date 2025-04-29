<script>
  import { goto } from "$app/navigation";
    import { supabase } from "$lib/supabase";
    
    let loading = false;

    async function signOut() {
        loading = true;
       const { error } = await supabase.auth.signOut()
       loading = false;
       if (error) {
           console.error('Error signing out:', error.message);
       } else {
           console.log('User signed out successfully');
           goto('/accounts/login');
       }
    }
</script>

<div>
    <button on:click={signOut} class="text-white text-sm p-2 w-68 mt-2 rounded-md bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">
        {#if loading}
            <svg class="animate-spin w-full" fill="#FFFFFF" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path d="M0 10.996c.484-5.852 5.145-10.512 10.996-10.996v2.009c-4.737.473-8.515 4.25-8.987 8.987h-2.009zm13.004-8.987c4.737.473 8.515 4.25 8.987 8.987h2.009c-.484-5.852-5.145-10.512-10.996-10.996v2.009zm-2.008 19.982c-4.737-.473-8.515-4.25-8.987-8.987h-2.009c.484 5.852 5.145 10.512 10.996 10.996v-2.009zm10.995-8.987c-.473 4.737-4.25 8.514-8.987 8.987v2.009c5.851-.484 10.512-5.144 10.996-10.996h-2.009z"/></svg>
        {:else}
            Log out
        {/if}
    </button>
</div>