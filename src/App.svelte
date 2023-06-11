<script>
    import { writable } from 'svelte/store';

    import Todo from './components/Todo.svelte';

    // Load the todos from local storage
    const storedTodos = localStorage.getItem('todos');
    const initialTodos = storedTodos ? JSON.parse(storedTodos) : [];

    // Create the writable store with the initial todos
    export const todos = writable(initialTodos); // Works as local state

    // Subscribe to the todos store and update local storage whenever it changes
    todos.subscribe((value) => {
        localStorage.setItem('todos', JSON.stringify(value));
    });

    const addTodo = () => {
        const newTodo = {
            id: Date.now(),
            task: '',
            completed: false,
            editing: true,
        };
        todos.update((t) => [...t, newTodo]);
    };

    const updateTodo = (id, task, completed) => {
        // Clear item on empty value
        if (!task) {
            todos.update((previousTodos) =>
                previousTodos.filter((todo) => (todo.id === id ? false : true))
            );
        } else {
            todos.update((previousTodos) =>
                previousTodos.map((todo) => {
                    if (todo.id === id) {
                        return { ...todo, task, completed, editing: false };
                    } else {
                        return todo;
                    }
                })
            );
        }
    };
</script>

<div class="container mx-auto">
    <h1 class="font-bold text-2xl mb-12">My Todos</h1>

    <div class="flex flex-col items-center">
        <div>
            <button
                class="bg-orange-500 py-2 text-white w-40 mb-6"
                on:click={addTodo}>Add Todo</button
            >
            <!-- <button
                class="bg-green-500 py-2 text-white w-24 mb-6 ml-3"
                on:click={handleRemember}>Remember</button
            > -->
        </div>

        {#each $todos as todo}
            <Todo
                task={todo.task}
                completed={todo.completed}
                editing={todo.editing}
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
