<script>
  let todos = []
  let newTodo = ''

  function handleAdd(event) {
    if (newTodo.trim() !== '' && event.key === 'Enter') {
      todos = [...todos, {
        text: newTodo,
        completed: false,
      }]
      newTodo = ''
    }
  }

  function handleComplete(idx) {
    todos = todos.map((todo, i) => {
      return { ...todo, completed: todo.completed || i === idx }
    })
  }

  function clearCompletedTodos() {
    todos = todos.filter(todo => !todo.completed)
  }

  $: sortedTodos = todos.sort((a, b) => a.completed === b.completed ? 0 : (a.completed ? 1 : -1))
</script>

<main>
  <h1>Todo App</h1>

  <ul>
    {#each sortedTodos as todo, idx}
      <li on:click="{e => handleComplete(idx)}">
        <p>
          {#if todo.completed}
            <s>{todo.text}</s>
          {:else}
            {todo.text}
          {/if}
        </p>
      </li>
    {:else}
      <li>
        There are no todos yet.
      </li>
    {/each}
  </ul>

  <input
    bind:value={newTodo}
    placeholder="add new task..."
    on:keyup={handleAdd}
  />
  <button on:click={clearCompletedTodos}>
    Clear completed todos
  </button>

</main>