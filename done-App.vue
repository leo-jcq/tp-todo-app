<script setup>
import { computed, ref } from 'vue';
import Todo from './components/Todo.vue';

const newTodo = ref('');

let id = 0;
const todos = ref([]);
const showCompleted = ref(true);

function addTodo() {
    todos.value.push({ id: id++, text: newTodo.value, done: false });
    newTodo.value = '';
}

function removeTodo(id) {
    todos.value = todos.value.filter((todo) => todo.id !== id);
}

function toggleDone(id) {
    const todo = todos.value.find((todo) => todo.id == id);

    if (todo) {
        todo.done = !todo.done;
    }
}

const filteredTodos = computed(() => {
    return showCompleted.value ? todos.value : todos.value.filter((todo) => !todo.done);
});
</script>

<template>
    <form @submit.prevent="addTodo">
        <input type="text" v-model="newTodo" />
        <button>Ajouter</button>
    </form>
    <ul>
        <li v-if="filteredTodos.length === 0">Aucune tâche à afficher</li>
        <Todo
            v-for="todo in filteredTodos"
            :key="todo.id"
            :todo="todo"
            @remove="removeTodo"
            @done="toggleDone"
        />
    </ul>
    <button @click="showCompleted = !showCompleted">
        {{ showCompleted ? 'Masquer' : 'Afficher' }} les tâches terminées
    </button>
</template>
