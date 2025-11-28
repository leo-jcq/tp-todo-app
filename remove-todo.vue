<script setup>
import { ref } from 'vue';

const newTodo = ref('');

let id = 0;
const todos = ref([]);

function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value });
    newTodo.value = '';
}

function removeTodo(id) {
    todos.value = todos.value.filter((todo) => todo.id !== id);
}
</script>

<template>
    <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" />
        <button>Ajouter</button>
    </form>
    <ul>
        <li v-if="todos.length === 0">Aucune tâche à afficher</li>
        <li v-for="todo in todos" :key="todo.id">
            {{ todo.text }}
            <button @click="removeTodo(todo.id)">Supprimer</button>
        </li>
    </ul>
</template>
