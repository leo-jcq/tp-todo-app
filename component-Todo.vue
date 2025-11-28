<script setup>
defineProps(['todo']);

defineEmits(['remove']);
</script>

<template>
    <li>
        {{ todo.text }}
        <button @click="$emit('remove', todo.id)">Supprimer</button>
    </li>
</template>
