<script>
    import Todo from './components/Todo.svelte';

    let todos = [{ id: 1, task: '', completed: false }];

    const addTodo = () => {
        const newTodo = { id: Date.now(), task: '', completed: false };
        todos = [...todos, newTodo];
    };

    const updateTodo = (id, task, completed) => {
        todos = todos.map((todo) => {
            if (todo.id === id) {
                return { ...todo, task, completed };
            } else {
                return todo;
            }
        });
    };
</script>

<div class="container mx-auto">
    <h1 class="font-bold text-2xl mb-12">My Todos</h1>

    <div class="flex flex-col items-center">
        <button
            class="bg-orange-700 px-4 py-2 text-white w-32 mb-6"
            on:click={addTodo}>Add Todo</button
        >
        {#each todos as todo (todo.id)}
            <Todo
                task={todo.task}
                completed={todo.completed}
                editing={true}
                on:update={(event) =>
                    updateTodo(
                        todo.id,
                        event.detail.task,
                        event.detail.completed
                    )}
            />
        {/each}
    </div>
</div>
