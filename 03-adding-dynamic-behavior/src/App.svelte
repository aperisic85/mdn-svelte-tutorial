<script>
  import Todos from './components/Todos.svelte';
  let todosForApp = [
    {id: 1, name: 'create a Svelte starter app', completed: true},
    {id: 2, name: 'complete your first component', completed: true},
    {id: 3, name: 'complete rest of tutorial', completed: false},
    {id: 4, name: 'go to sleep', completed: false},
  ]
</script>

<Todos todos = {todosForApp} />