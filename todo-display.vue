<script setup>
import { ref } from 'vue';

const newTodo = ref('');

let id = 0;
const todos = ref([]);

function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value });
    newTodo.value = '';
}
</script>

<template>
    <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" />
        <button>Ajouter</button>
    </form>
    <ul>
        <li v-if="todos.length === 0">Aucune tâche à afficher</li>
        <li v-for="todo in todos" :key="todo.id">{{ todo.text }}</li>
    </ul>
</template>
