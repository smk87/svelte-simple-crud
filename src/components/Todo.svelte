<script>
    export let task;
    export let completed = false;
    export let editing = false;

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
        const event = new CustomEvent('update', {
            detail: {
                task,
                completed,
            },
        });
        dispatchEvent(event);
    };
</script>

{#if editing}
    <input
        class="border-2 rounded border-orange-700 active:border-orange-300 hover:border-orange-300 outline-none mb-4"
        type="text"
        bind:value={task}
        on:blur={stopEditing}
        placeholder="Enter your todo here"
    />
{:else}
    <div class={completed ? 'completed' : ''} on:dblclick={startEditing}>
        <input
            type="checkbox"
            on:change={toggleCompleted}
            checked={completed}
        />
        <span>{task}</span>
    </div>
{/if}
