<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import type { Todo } from '../types';
  
  export let todos: Todo[] = [];
  let newTodoText = '';
  
  const dispatch = createEventDispatcher();
  
  function updateParent() {
    dispatch('update', todos);
  }
  
  function addTodo(): void {
    if (newTodoText.trim() === '') {
      return;
    }
    const newTodo: Todo = {
      id: Date.now().toString(),
      text: newTodoText,
      isEditing: false
    };
    todos = [...todos, newTodo];
    newTodoText = '';
    updateParent();
  }
  
  function toggleTodoEdit(id: string): void {
    todos = todos.map(todo => ({
      ...todo,
      isEditing: todo.id === id ? !todo.isEditing : todo.isEditing
    }));
    updateParent();
  }
  
  function updateTodoText(event: Event, id: string): void {
    const target = event.target as HTMLInputElement;
    todos = todos.map(todo => {
      if (todo.id === id) {
        return { ...todo, text: target.value };
      }
      return todo;
    });
    updateParent();
  }
  
  function handleDelete(id: string): void {
    todos = todos.filter(todo => todo.id !== id);
    updateParent();
  }
  </script>
  
  <style>
  .todo-add-form {
    margin-bottom: 20px;
  }
  .todo-list {
    list-style-type: none;
    padding: 0;
  }
  .todo-item {
    margin-bottom: 10px;
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