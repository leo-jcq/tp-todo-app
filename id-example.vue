<script setup>
import { ref } from 'vue';

// Compteur pour générer des IDs uniques
let id = 0;

// Exemple de tableau d'éléments avec IDs
const items = ref([]);

function addItem() {
    // Quand on ajoute un nouvel élément, on utilise id++
    // Cela donne la valeur actuelle de id, puis l'incrémente
    items.value.push({ id: id++, name: `Élément ${id}` });
}
</script>

<template>
    <button @click="addItem">Ajouter un élément</button>
</template>
