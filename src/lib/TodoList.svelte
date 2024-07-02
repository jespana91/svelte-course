<svelte:options immutable={true}/>

<script>
    import Button from "./Button.svelte";
    import FaPlusCircle from 'svelte-icons/fa/FaPlusCircle.svelte'
    import { createEventDispatcher, onMount, onDestroy, beforeUpdate, afterUpdate } from "svelte";

    onMount(() => {
        console.log("Mounted");
        return () => {
            console.log("Destroyed from Mount");
        }
    });

    onDestroy(() => {
        console.log("Destroyed");
    })

    beforeUpdate(() => {
        if(listDiv)
            console.log(listDiv.offsetHeight);
    })

    afterUpdate(() => {
        console.log(listDiv.offsetHeight);
    })

    export let todos = [];    
    export const readOnly = 'read only';
    export function clearInput() {
        inputText = '';
    }
    // export function focusInput() {
    //     input.focus();
    // }
    
    let inputText = '';
    let input, listDiv;

    const dispatch = createEventDispatcher();
    
    function handleAddTodo() {
        const isNotCancelled = dispatch(
            'addTodo', 
            {
                title: inputText
            },
            {
                cancelable: true
            }
        );

        if(isNotCancelled) {
            inputText = '';
        }
        
    }

    function handleRemoveTodo(id) {
        dispatch(
            'removeTodo', 
            {
                id
            }
        );
    }

    function handleToggleTodo(id, value) {

        dispatch(
            'toggleTodo', 
            {
                id, 
                value
            }
        );
    }
</script>

<div class="todo-list-wrapper">
    <div class="todo-list" bind:this={listDiv}>
        <!-- display the list -->
        <ul>
            {#each todos as {id, title, completed} (id)}
            <!-- {@debug id, title} -->
                <!-- {@const number = index + 1} -->
                <li>
                    
                    <label>
                        <input 
                            on:input={(event) => {
                                event.currentTarget.checked = completed;
                                handleToggleTodo(id, !completed);
                            }} 
                            type="checkbox" 
                            checked={completed}
                        />
                        {title}
                    </label>
                    <button on:click={() => handleRemoveTodo(id)}>Remove</button>
                    
                </li>
            {/each}
            
        </ul>
    </div>
    <!-- show the input text dynamically -->
    {inputText}

    <!-- form to add new todos -->
    <form class="add-todo-form" on:submit|preventDefault={handleAddTodo}>

        <input bind:this={input} bind:value={inputText}/>
        
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