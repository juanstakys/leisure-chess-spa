<script setup>
import { ref, computed } from "vue";
import Home from "./Home.vue";
import Play from "./Play.vue";
import NotFound from "./NotFound.vue";

const routes = {
    "/": Home,
    "/play": Play,
};

const currentPath = ref(window.location.hash);

window.addEventListener("hashchange", () => {
    currentPath.value = window.location.hash;
});

const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || "/"] || NotFound;
});
</script>

<template>
    <h1>Chess Now!</h1>
    <component :is="currentView" />
</template>

<style scoped></style>
