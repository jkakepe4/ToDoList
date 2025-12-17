<script>
    import { preventDefault } from "svelte/legacy";
    import {onMount} from 'svelte';

    let todoItem = $state('');
    let todoDate = $state();
    let todoList = $state([]);
    let todayDate = $state();

    onMount(() => {
        let storedList = localStorage.getItem("storedList");
        if (storedList) {
            todoList = (JSON.parse(storedList));
        }

        checkDueDates();
    })

    function updateList() {
        return localStorage.setItem("storedList", JSON.stringify(todoList));
    }

    // add todoItem to todoList
    function addItem(event) {
        event.preventDefault();
        if (todoItem.trim().length === 0) {
            return;
        }
        let itemDueDate = Math.floor(new Date(todoDate).getTime() / 1000);
        console.log(itemDueDate);
        todoList = [...todoList, {
            text: todoItem,
            dueDate: itemDueDate,
            dueSoon: false,
            late: false,
            done: false
        }];
        updateList();
        todoItem = '';
        todoDate = '';
    }

    // remove todoItem from todoList
    function removeItem(index) {
        todoList.splice(index,1);
        updateList();
    }

    function clearAll() {
        todoList = '';
        localStorage.clear();
    }

    function checkDueDates() {
        todayDate = Math.floor(new Date().getTime() /1000);

        todoList.forEach(item => {
            let daysDue = calculateDelta(todayDate, item.dueDate);
            if (daysDue < 7) {
                item.dueSoon = true;
            }
            if (daysDue < 0) {
                item.late = true;
            }
        })
    }

    function calculateDelta(date1, date2) {
        return Math.round((date2 - date1) / 86400);
    }

    function clearDone() {
        todoList = todoList.filter(item => !item.done);
        updateList();
    }

    function scrollToBottom() {
        window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' });
    }

</script>

    <form onsubmit={addItem}>
        <input type="text" bind:value={todoItem}>
        <input type="date" bind:value={todoDate}>
        <button type="submit">Add</button>

    </form>

    <div class="todo-list">
        <ul>
            {#each todoList as item, index}
                <li>
                    <input type="checkbox" bind:checked={item.done} onchange= {updateList}>
                    <span class:done={item.done} class:late={item.late} class:soon={item.dueSoon}>{item.text}
                            {#if item.dueDate}
                        {#if item.late}
                            Late!
                        {:else if item.dueSoon}
                            Due Soon!
                        {:else}
                            due in {calculateDelta(todayDate, item.dueDate)} days.
                        {/if}
                            {/if}
                    </span>
                    <button type="button" onclick= {() => removeItem(index)}>&times;</button>
                </li>
            {/each}
        </ul>
    </div>

    {#if todoList.length > 0}
    <button class="clearAll" onclick={clearAll}>Clear All</button>
    {:else}
    <button class="clearAll" disabled>Clear All</button>
    {/if}

    <button onclick={clearDone}>Clear Done</button>
    
    <button class="scroll-bottom" onclick={scrollToBottom}>⬇</button>

<style>
    ul {
        list-style: none;
    }
    input[type="checkbox"] {
        height: 20px;
        width: 20px;
        accent-color: rgb(6, 110, 255);
    }
    li span.done {
        text-decoration: line-through;
        color: grey;    
    }
    li span.late.soon {
        color: red;
    }
    li span.soon {
        color: goldenrod;
    }
    li span {
        max-width: 45ch;
        word-wrap: break-word;
        overflow-wrap: break-word;
        
    }
    .scroll-bottom {
        position: fixed;
        bottom: 20px;
        right: 20px;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background-color: #157382;
        color: white;
        border: none;
        font-size: 24px;
        cursor: pointer;
        box-shadow: 0 2px 5px rgba(0,0,0,0.3);
    }
    .scroll-bottom:hover {
        background-color: #a2e5ef;
        color: #157382;
        transition: 250ms;
    }
</style>