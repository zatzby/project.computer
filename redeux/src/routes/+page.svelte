<script lang="ts">
    import TodoList from '../lib/components/TodoList.svelte'; // Adjust the path as necessary
    import type { Todo } from '../lib/types';

    type TodoListType = {
      id: string;
      title: string;
      todos: Todo[];
      isEditing: boolean; // Property to track editing state
    };
  
    let todoLists: TodoListType[] = [
      { id: 'list-1', title: 'My First List', todos: [], isEditing: false }
    ];
  
    function addNewList(): void {
      const newList: TodoListType = {
        id: `list-${todoLists.length + 1}`,
        title: `New List ${todoLists.length + 1}`,
        todos: [],
        isEditing: false
      };
      todoLists = [...todoLists, newList];
    }
  
    function deleteList(listId: string): void {
      todoLists = todoLists.filter(list => list.id !== listId);
    }
  
    function toggleEditListName(listId: string): void {
      todoLists = todoLists.map(list => {
        if (list.id === listId) {
          return { ...list, isEditing: !list.isEditing };
        }
        return list;
      });
    }
  
    function updateListName(event: Event, listId: string): void {
      const target = event.target as HTMLInputElement;
      todoLists = todoLists.map(list => {
        if (list.id === listId) {
          return { ...list, title: target.value };
        }
        return list;
      });
    }
  </script>
  
  <style>
    h1 {
      color: #333;
      text-align: center;
    }
  
    /*.todo-list-container {
      max-width: 600px;
      margin: 20px auto;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      background-color: #f9f9f9;
    }*/
  
    button {
      background-color: #007bff;
      color: white;
      padding: 8px 15px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin-right: 5px;
    }
  
    button:hover {
      background-color: #0056b3;
    }
  
    input[type="text"] {
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      margin-right: 5px;
    }
  
    h2 {
      color: #555;
      margin-bottom: 10px;
    }
  </style>
  

  <h1>ListChamp</h1>
  
  <button on:click={addNewList}>Add New List</button>
  
  {#each todoLists as todoList}
    <div>
      {#if todoList.isEditing}
        <input
          type="text"
          value={todoList.title}
          on:input={(event) => updateListName(event, todoList.id)}
        />
        <button on:click={() => toggleEditListName(todoList.id)}>Save</button>
      {:else}
        <h2>
          {todoList.title} 
          <button on:click={() => toggleEditListName(todoList.id)}>Edit List Name</button>
        </h2>
      {/if}
      <TodoList todos={todoList.todos} />
      <button on:click={() => deleteList(todoList.id)}>Delete List</button>
    </div>
  {/each}
  