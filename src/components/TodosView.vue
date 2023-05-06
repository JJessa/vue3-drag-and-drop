<script setup>
import { reactive } from 'vue';
import InputNew from './InputNew.vue'

let boards = reactive([
    {
        id: crypto.randomUUID(),
        name: 'Board 1',
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Feature de archivos',
            },

            {
                id: crypto.randomUUID(),
                title: 'Resolver bug',
            },
        ],
    },

    {
        id: crypto.randomUUID(),
        name: 'Board 2',
        items: [
            {
                id: crypto.randomUUID(),
                title: 'Mandar reporte',
            },

            {
                id: crypto.randomUUID(),
                title: 'Code review',
            },
        ],
    },
]);


function handleNewItem(text, board){
    console.log(text.value, board.id, board.name);
    board.items.push({
        id: crypto.randomUUID(),
        title: text.value,
    });
}

function handleNewBoard(){
    const name = prompt('Name of the board');
    if(!!name){ //Esto es igual a if(name !== " ")
            boards.push({
                id: crypto.randomUUID(),
            name: name,
            items: [],
        });
    }
}

function startDrag(e, board, item){
    e.dataTransfer.setData("text/plain", JSON.stringify({boardId: board.id, itemId: item.id}))
}

function onDrop(e, destiny){
    const {boardId, itemId} = JSON.parse(e.dataTransfer.getData("text/plain"));
    console.log(boardId, itemId);
    const originBoard = boards.find(item => item.id === boardId);
    const originItem = originBoard.items.find(item => item.id === itemId);

    destiny.items.push({...originItem});
    originBoard.items = originBoard.items.filter((item) => item !== originItem)
}
</script>

<template>
<nav>
    <ul>
        <li><a href='#' @click.prevent="handleNewBoard">Create new board</a></li>
    </ul>

</nav>

<div class="boards-container">
    <div class="boards">
        <div class="board" @drop="onDrop($event, board)" @dragover.prevent @dragenter.prevent v-for="board in boards" :key="board.id">
            <div class="boardTitle">{{ board.name }}</div>

            <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
            <div class="items">
                <div class="item" draggable="true" @dragstart="startDrag($event, board, item)" v-for="item in board.items" :key="item.id">
                {{ item.title }}
                </div>
        </div>

        </div>
    </div>
</div>
</template>


<style scope>
*{
    padding: 0.2rem;
}
nav{
    background-color: purple; 
    margin-bottom: 0.625rem;  
}

nav ul{
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
}

nav ul li a{
    display: block;
    padding: 0.625rem;
    color: white;
    text-decoration: none;
    font-size: 1.6rem;
}

.boardTitle{
    margin-bottom: 0.625rem;
    font-size: 1.3rem;
    color: #4A235A;
    font-weight: 700;
}

.boards{
    display: flex;
    gap: 0.625rem;
}

.board{
    background: #efefef;
    padding: 0.625rem;
}

.items{
    display: flex;
    flex-direction: column;
    gap: 0.313rem;
}
.item{
    background-color: white;
    padding: 0.625rem;
    box-sizing: border-box;
}
</style>