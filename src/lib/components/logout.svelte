<script>
  import { goto } from "$app/navigation";
    import { supabase } from "$lib/supabase";
    
    async function signOut() {
       const { error } = await supabase.auth.signOut()
       if (error) {
           console.error('Error signing out:', error.message);
       } else {
           console.log('User signed out successfully');
           goto('/accounts/login');
       }
    }
</script>

<div>
    <button on:click={signOut} class="text-white text-sm p-2 w-68 mt-2 rounded-md bg-sky-500 hover:bg-blue-500 hover:cursor-pointer font-[600]">Log out</button>
</div>