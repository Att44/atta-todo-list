<script>

    import supabase from '$lib/db';
       var item = "";
    
       async function saveEntry() {
            const { error } = await supabase.from('itemEntries').insert(
                {
                    user_id: supabase.auth.user().id,
                    item: item
                }
            );
            if (error) alert(error.message);
    
            location.reload(); // Refresh the page.
        }
    
        
        function checkEntry() {
            if (item === ""){
                alert("Please Enter Activity")
            }
            else{
                saveEntry()
            }
        }
      
    </script>
    
    <div class="container text-center">
      <input bind:value={item} type="text" class="form-control m-3">
      <button type="button" class="btn btn-primary mb-3" on:click={checkEntry}>Add</button>
    </div>
    
    