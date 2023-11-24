<!-- Todos.svelte -->
<script>
  export let todos = []
  // make variable reactive by replacing 'let' with '$'
  //'$' means re-run this code whenever any of referenced values change
  $: totalTodos = todos.length;
  $: completedTodos = todos.filter(x => x.completed).length;
  function removeTodo(todo) {
    //update todo array by leaving all todos that are not parameter id
    todos = todos.filter(x => x.id !== todo.id);
  }
  let newTodoName = "";
  let newTodoId;
  $: {
    if (totalTodos == 0){
      newTodoId = 1;}
      else{
        newTodoId = Math.max(...todos.map(t => t.id))+1;
      }
    
  }

  function addTodo(){
    //todos.push({id: 99, name: newTodoName, completed: false });
    //todos = todos; // asigment to trigger reactivity to update UI
    todos = [...todos, {id: newTodoId, name: newTodoName, completed: false }]; // to replace above

    newTodoName = "";
  }

let filter = "All";
const filterTodos = (filter, todos) =>
  filter === "active" 
    ? todos.filter(t => !t.completed) 
    : filter ==="completed" 
      ? todos.filter(t=>t.completed)
      :todos;

</script>


<div class="todoapp stack-large">

  <!-- NewTodo -->
  <form on:submit|preventDefault={addTodo}>
    <h2 class="label-wrapper">
      <label for="todo-0" class="label__lg">
        What needs to be done?
      </label>
    </h2>
    <input bind:value={newTodoName}  type="text" id="todo-0" autocomplete="off"
      class="input input__lg" />
    <button type="submit" disabled="" class="btn btn__primary btn__lg">
      Add
    </button>
  </form>

  <!-- Filter -->
  <div class="filters btn-group stack-exception">
    <button class="btn toggle-btn" aria-pressed={filter === 'all'} class:btn-primary = {filter ==='all'} on:click={() => filter = 'all'} >
      <span class="visually-hidden">Show</span>
      <span>All</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button class="btn toggle-btn" aria-pressed={filter === 'active'}  class:btn-primary = {filter ==='active'} on:click={() => filter = 'active'}>
      <span class="visually-hidden">Show</span>
      <span>Active</span>
      <span class="visually-hidden">tasks</span>
    </button>
    <button class="btn toggle-btn" aria-pressed={filter === 'completed'}  class:btn-primary = {filter ==='completed'} on:click={() => filter = 'completed'}>
      <span class="visually-hidden">Show</span>
      <span>Completed</span>
      <span class="visually-hidden">tasks</span>
    </button>
  </div>

  <!-- TodosStatus -->
  <h2 id="list-heading">{completedTodos} out of {totalTodos} completed</h2>

  <!-- Todos -->
  <ul role="list" class="todo-list stack-large" aria-labelledby="list-heading">
    {#each filterTodos(filter,todos) as todo, index (todo.id) }
        <li class="todo">
          <div class="stack-small ">
            <div class="c-cb">
              <input type="checkbox" id="todo-{todo.id}" 
                on:click={() => todo.completed = !todo.completed} 
                checked={todo.completed}/>
              <label for="todo-{todo.id}" class="todo-label">
                {todo.name}
              </label>
            </div>
            <div class="btn-group">
              <button type="button" class="btn font-monospace ">
                Edit <span class="visually-hidden">{todo.name}</span>
              </button>
              <button type="button" class="btn btn__danger" on:click={() => removeTodo(todo)}>
                Delete <span class="visually-hidden">{todo.name}</span>
              </button>
            </div>
          </div>
      </li>
    {:else} Nothing to do here!  
    {/each}
  </ul>
  <hr />

  <!-- MoreActions -->
  <div class="btn-group">
    <button type="button" class="btn btn__primary">Check all</button>
    <button type="button" class="btn btn__primary">Remove completed</button>
  </div>

</div>