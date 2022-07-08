<script>
  import { Router, Route, Link } from "svelte-navigator";
  import Main from './components/Main.svelte';
  import About from './components/About.svelte';
  let todos = [];
  let name = '';
  let _allTodos = [];

  function deleteTodo(i) {
    todos = todos.filter((_, index) => index !== i);
    _allTodos = todos;
  }

  function addTodo(name) {
    todos = todos.concat([{name, id: new Date().getTime()}]);
    _allTodos = todos;
  }

  function search(e) {
    const query = e.target.value;
    todos = _allTodos.filter(item => item.name.toLocaleLowerCase().includes(query));
  }
</script>
<Router>
  <main>
    <header>
        <nav>
          <Link to="/">
            <button>Main</button>
          </Link>
          <Link to="about">
            <button>About</button>
          </Link>
        </nav>
    </header>

    <h1>Todo List</h1>

    <Route path="/">
      <Main />
    </Route>
    <Route path="about">
      <About />
    </Route>

    <div style="display: flex; margin-bottom: 20px;">
      <div>
        <input type="text" bind:value="{name}" placeholder="ToDo name" maxlength="50">
        <p style="color: {name?.length < 50 ? 'grey' : 'red'}; margin: 0">{name?.length} / 50</p>
      </div>
      <button class="button" on:click="{() => addTodo(name)}" disabled="{!name?.length}">Create</button>
    </div>
    <input style="margin-bottom: 10px" type="text" placeholder="search..." on:input="{(e) => search(e)}">

    {#each todos as todo, i}
      <article class="todo-item">
        <span>{ todo.name }</span>
        <span on:click="{() => deleteTodo(i)}">X</span>
      </article>
    {/each}
  </main>
</Router>
<style>
  * {
    box-sizing: border-box;
  }

  main {
    font-family: sans-serif, Arial, Helvetica;
    margin: 0;
    height: 100%;
    padding: 20px;
  }

  header {
    margin-bottom: 20px;
  }

  nav button {
    border: 1px solid #ccc;
    outline: none;
    border-radius: 4px;
    background-color: #fff;
    padding: 14px 24px;
    font-size: 14px;
    margin-right: 20px;
    cursor: pointer;
  }

  nav button:hover {
    background-color: #ccc;
  }

  .todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 500px;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 18px;
    margin-bottom: 20px;
  }

  input {
    border-radius: 4px;
    height: 40px;
    border: 1px solid #ccc;
    margin-right: 20px;
  }

  .button {
    height: 40px;
    border: 1px solid #ccc;
    outline: none;
    border-radius: 4px;
    background-color: #fff;
    color: #363636;
    cursor: pointer;
  }
  .button:hover {
    background-color: #ccc;
  }
  .button:disabled {
    opacity: .5;
    pointer-events: none;
  }
</style>
