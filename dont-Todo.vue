<script setup>
defineProps({
    todo: {
        type: Object,
        required: true
    }
});

defineEmits(['remove', 'done']);
</script>

<template>
    <li :class="{ done: todo.done }">
        {{ todo.text }}
        <input type="checkbox" :checked="todo.done" @change="$emit('done', todo.id)" />
        <button @click="$emit('remove', todo.id)">Supprimer</button>
    </li>
</template>
