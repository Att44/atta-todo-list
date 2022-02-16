<script>
    import Navbar from '$lib/Navbar.svelte';
    import Todo from '$lib/Todo.svelte';
    import Greeting from '$lib/Greeting.svelte';
    import supabase from '$lib/db';

    async function signOut() {
   	 const { error } = await supabase.auth.signOut();

   	 if (error) alert(error.message); // alert if error
    }

</script>


<Navbar/>
<Greeting/>
<Todo/>

<!-- Sign Out -->
<section class="container px-4 py-3 text-center">
    <button class="btn btn-secondary" on:click={signOut}>Logout</button>
</section>

