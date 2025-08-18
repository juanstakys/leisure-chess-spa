<template>
    <!-- TODO: Rename this as it's not a modal -->
    <div class="modal">
        <p>Create game as</p>
        <div>
            <button @click="handleClick" value="random">Random</button>
            <button @click="handleClick" value="white">White</button>
            <button @click="handleClick" value="black">Black</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits<{
    (e: "color-selected", color: string): void;
}>();

const handleClick = (event) => {
    const value = event.target.value;
    emit("color-selected", value);
    // Handle the color selection logic here
};

const open = ref(false);
</script>

<style scoped>
.modal {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 300px;
    border-color: #ccc;
    border-style: solid;
    border-width: 1px;
    border-radius: 15px;
    padding: 10px;
    background-color: #555;
}
.modal div button {
    margin: 5px;
}
</style>
