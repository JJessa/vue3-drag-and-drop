<script setup>
import { ref, defineEmits } from "vue";

const text = ref("");
const emits = defineEmits(["onNewItem"]);

function handleSubmit(){
    if(text !== ''){
        emits("onNewItem", text); //Aquí es donde llamamos a la función del emit
        text.value = "";
    }
}
</script>

<template>
<form action="" @submit.prevent="handleSubmit">
    
    <input type="text" v-model="text">

</form>
</template>

<style scope>

form{
    margin-bottom: 0.625rem;
}

input{
    width: 100%;
    box-sizing: border-box;
}

</style>