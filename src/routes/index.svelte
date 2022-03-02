<script>
    import Navbar from '$lib/Navbar.svelte';
    import Todo from '$lib/Todo.svelte';
    import Greeting from '$lib/Greeting.svelte';
    import supabase from '$lib/db';


    async function signOut() {
   	 const { error } = await supabase.auth.signOut();

   	 if (error) alert(error.message); // alert if error
    }

    async function getEntries() {
   	 const { data, error } = await supabase.from('itemEntries').select();
   	 if (error) alert(error.message);

   	 return data;
    }

	async function deleteEntry(x) {
   	 const { error } = await supabase.from('itemEntries').delete().match(
			{
				item: x
			}
		)

   	 if (error) alert(error.message);

   	 location.reload(); // Refresh the page.
    }

	

</script>


<Navbar/>
<Greeting/>
<!-- <Todo/> -->

<Todo/>

    {#await getEntries()}
   		 <p class="text-center">Fetching data...</p>
   	 {:then data}
		<div class="container bg-light rounded-3 p-2 h4 w-50">
   		 	{#each data as entry}
				<table class="table">
					<tr>
						<td  style="width: 100%;">
							<li>{entry.item}</li>
						</td>
						<td>
							<div><button class="btn btn-danger" on:click={()=>{deleteEntry(entry.item)}}>x</button></div>
						</td>
					</tr>
				</table>
				
   		 	{/each}
		</div>
   	 {:catch error}
   		 <p>Something went wrong while fetching the data:</p>
   		 <pre>{error}</pre>
   	 {/await}

<!-- Sign Out -->
<section class="container px-4 py-3 text-center">
    <button class="btn btn-secondary" on:click={signOut}>Logout</button>
</section>

