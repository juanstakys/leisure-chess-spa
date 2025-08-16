<template>
    <h1>Chess Now!</h1>
    <component
        :is="currentView"
        :gameId="gameId"
        @create="onCreate"
        @join="onJoin"
    />
</template>

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

let gameId;

window.addEventListener("hashchange", () => {
    currentPath.value = window.location.hash;
});

const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || "/"] || NotFound;
});

async function onCreate() {
    const response = await fetch("http://localhost:3000");
    gameId = await response.text();
    window.location.hash = "/play";
}

async function onJoin(gId) {
    gameId = gId;
    window.location.hash = "/play";
}
</script>

<style scoped></style>
