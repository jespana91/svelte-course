<svelte:options immutable={true}/>

<script>
    import Button from "./Button.svelte";
    import FaPlusCircle from 'svelte-icons/fa/FaPlusCircle.svelte'
    import { createEventDispatcher } from "svelte";

    export let todos = [];    
    let inputText = '';

    const dispatch = createEventDispatcher();
    
    function handleAddTodo() {
        dispatch('addTodo', {
            title: inputText
        })
        inputText = '';
    }

    function handleRemoveTodo(id) {
        dispatch('removeTodo', {
            id
        })
    }

    function handleToggleTodo(id, value) {
        dispatch('toggleTodo', {
            id, value
        })
    }
</script>

<div class="todo-list-wrapper">

    <!-- display the list -->
    <ul>
        {#each todos as {id, title, completed} (id)}
            <!-- {@const number = index + 1} -->
            <li>
                
                <label>
                    <input on:input={(event) => {
                        event.currentTarget.checked = completed;
                        handleToggleTodo(id, !completed);
                    }} type="checkbox" checked={completed}>
                    {title}
                </label>
                <button on:click={() => handleRemoveTodo(id)}>Remove</button>
                
            </li>
        {/each}
        
    </ul>

    <!-- show the input text dynamically -->
    {inputText}

    <!-- form to add new todos -->
    <form class="add-todo-form" on:submit|preventDefault={handleAddTodo}>

        <input bind:value={inputText}/>
        
        <Button type="submit" disabled={!inputText}>
            <div style:width="20px" slot="leftContent">
                <FaPlusCircle/>
            </div>
            Add
        </Button>

    </form>
</div>

<style>

</style>