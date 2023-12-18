<script lang="ts">
    import type { Todo } from '../types'; // Assuming you have a types.ts file
    import { createEventDispatcher } from 'svelte';
  
    // Define the type for the 'todo' prop
    export let todo: Todo;
  
    const dispatch = createEventDispatcher();
  
    function deleteTodo(): void {
      dispatch('delete', todo.id);
    }
  
    function editTodo(): void {
      dispatch('edit', todo.id);
    }
  </script>
  
  <div>
    <span>{todo.text}</span>
    <button on:click={deleteTodo}>Delete</button>
    <button on:click={editTodo}>Edit</button>
  </div>
  