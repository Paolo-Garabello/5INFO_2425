<script>
    import { onMount } from "svelte";
    import Icon from "./Icon.svelte";
    import ToDoItem from "./ToDo-Item.svelte";
    let todos = [];
    let last_id = 0;

    const createTodo = async () => {
        let todo = {
            id: ++last_id,
            task: '',
            done: false,
            priority: 3
        };
        console.log("created + "+ todo);

        localStorage.setItem(`todo${todo.id}`, JSON.stringify(todo))
        todos = [...todos, todo];
    }
    const change_todo_item = async (e) => {
        delete_item(e.detail.id);
    }
    const delete_item = (id) => {
        console.log("DELETE", id);
        todos = todos.filter(t => t.id != id);
    } 

    onMount(async () => {
        for(let i = 0; i < localStorage.length; i++) {
            const key = localStorage.key(i);
            const todo = JSON.parse(localStorage.getItem(key));
            if(todo != null) 
                todos.push(todo)
        }
        todos = [...todos];
    })
</script>

<h1 class="app-title">Todos</h1>
<div class="todo-list">
    <div class="header"><Icon name="tag"/></div>
    <div class="header"><Icon name="task_alt"/></div>
    <div class="header"><Icon name="list"/></div>
    <div class="header"><Icon name="schedule"/></div>
    <div class="header header-last"><Icon name="add_box" handler={createTodo}/></div>
    {#each todos as todo}
        <ToDoItem todo={todo} on:change={change_todo_item}/>
    {/each}
</div>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap");

    .todo-list {
        border: 0px solid blue;
        width: 95%;
        margin: auto;
        display: grid;
        grid-template-columns: 1fr 1fr 4fr 2fr 1fr;
    }

    .header {
        border-bottom: 1px solid #E7ECEE;
        border-right: 1px solid #E7ECEE;
        text-align: center;
        padding-bottom: 20px;
    }

    .header-last {
        border-right: none;
    }

    .app-title {
        font-family: 'Permanent Marker', cursive;
        margin-top: 0px;
        font-size: 60px;
        opacity: 0.3;
    }
</style>