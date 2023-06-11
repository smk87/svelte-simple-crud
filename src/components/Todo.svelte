<script>
    import { createEventDispatcher } from 'svelte';

    // Works as props
    export let task = '';
    export let completed = false;
    export let editing = false;

    const dispatch = createEventDispatcher();

    const toggleCompleted = () => {
        completed = !completed;
        dispatchUpdateEvent();
    };

    const startEditing = () => {
        editing = true;
    };

    const stopEditing = () => {
        editing = false;
        dispatchUpdateEvent();
    };

    const dispatchUpdateEvent = () => {
        dispatch('update', {
            task,
            completed,
        });
    };

    const handleKeyDown = (event) => {
        if (event.key === 'Enter') {
            stopEditing();
        }
    };

    const handleDelete = () => {
        dispatch('update', {
            task: '',
            completed,
        });
    };
</script>

{#if editing}
    <div class="flex justify-between items-center">
        <!-- svelte-ignore a11y-autofocus -->
        <input
            class="border-2 rounded border-orange-700 active:border-orange-300 hover:border-orange-300 outline-none mb-4 w-48 px-2"
            type="text"
            bind:value={task}
            on:keydown={handleKeyDown}
            placeholder="Enter your todo here"
            on:change={dispatchUpdateEvent}
            autofocus
        />
    </div>
{:else}
    <div class="my-2" on:dblclick={startEditing}>
        <input
            type="checkbox"
            on:change={toggleCompleted}
            checked={completed}
        />
        <span class={`cursor-pointer p-2 ${completed ? 'line-through' : ''}`}
            >{task}</span
        >
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <span
            class="ml-4 mb-4 py-1 px-2 bg-red-700 rounded-md shadow-lg cursor-pointer"
            on:click={handleDelete}
        >
            Delete
        </span>
    </div>
{/if}
