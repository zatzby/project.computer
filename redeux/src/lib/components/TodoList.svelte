<script lang="ts">
  import type { Todo } from '../types';


  export let todos: Todo[] = []; // Array of todos
  let newTodoText = ''; // Text for new todo input field

  // Function to add a new todo
  function addTodo(): void {
    if (newTodoText.trim() === '') {
      return; // Avoid adding empty todos
    }
    const newTodo: Todo = {
      id: Date.now().toString(), // Unique id generation
      text: newTodoText,
      isEditing: false
    };
    todos = [...todos, newTodo];
    newTodoText = ''; // Reset input field
  }

  // Function to toggle editing state of a todo
  function toggleTodoEdit(id: string): void {
    todos = todos.map(todo => ({
      ...todo,
      isEditing: todo.id === id ? !todo.isEditing : todo.isEditing
    }));
  }

  // Function to update a todo's text
  function updateTodoText(event: Event, id: string): void {
    const target = event.target as HTMLInputElement;
    todos = todos.map(todo => {
      if (todo.id === id) {
        return { ...todo, text: target.value };
      }
      return todo;
    });
  }

  // Function to handle deletion of a todo
  function handleDelete(id: string): void {
    todos = todos.filter(todo => todo.id !== id);
  }

</script>

<style>
  /* Existing styles... */
  .todo-add-form {
    margin-bottom: 20px;
  }
  
  .todo-item {
    /* Style for todo items */
  }
</style>

<div class="todo-add-form">
  <input type="text" bind:value={newTodoText} placeholder="Add new todo" />
  <button on:click={addTodo}>Add Todo</button>
</div>

<ul class="todo-list">
  {#each todos as todo}
    <li class="todo-item">
      {#if todo.isEditing}
        <input
          type="text"
          value={todo.text}
          on:input={(event) => updateTodoText(event, todo.id)}
        />
        <button on:click={() => toggleTodoEdit(todo.id)}>Save</button>
      {:else}
        <span>{todo.text}</span>
        <button on:click={() => toggleTodoEdit(todo.id)}>Edit</button>
        <button on:click={() => handleDelete(todo.id)}>Delete</button>
      {/if}
    </li>
  {/each}
</ul>
