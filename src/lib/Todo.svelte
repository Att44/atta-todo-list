<script>
    import supabase from '$lib/db';

    let newItem = '';
	
    let todoList = [
        {text: {newItem}, status: false}
    ]

	function addToList() {
		todoList = [...todoList, {text: newItem, status: false}];
		newItem = '';
        saveEntry();
	}
	
	function removeFromList(index) {
		todoList.splice(index, 1)
		todoList = todoList;
        saveEntry();
    }

    function setCheck(){
        
    }

    async function saveEntry() {
   	 const { error } = await supabase.from('itemEntries').insert(
   		 {
   			 user_id: supabase.auth.user().id,
   			 todo_list: newItem
   		 }
   	 );
   	 if (error) alert(error.message);

   	 location.reload(); // Refresh the page.
    }
</script>

<div class="container bg-light p-5 rounded">
	<input bind:value={newItem} class="form-control form-control-lg" type="text" placeholder="new todo item..">
    <div class="text-center"><button class="btn btn-success btn-lg m-5 text-center" on:click={addToList}>Add</button> <br>
    
        {#each todoList as item, index}
	    <input class="custom-control custom-checkbox checkbox-xl" bind:checked={item.status} type="checkbox">
	    <span class="h3" class:checked={item.status}>{item.text}</span>
	    <span class="h5" on:click={() => removeFromList(index)}>❌</span>
        <br>
        {/each} 
    </div>
	

<style> 
	.checked {
        text-decoration: line-through;
    }

    .h3 {
        width:30px;
        height:30px;
    }
</style> 
</div>