<template>
    <h1>Chess Now!</h1>
    <Home @create="onCreate" @join="onJoin" v-if="!gameId" />
    <Play
        :gameId="gameId"
        :player-color="playerColor"
        :ws="ws"
        v-else-if="playerColor"
    />
</template>

<script setup>
import { ref } from "vue";
import Home from "./Home.vue";
import Play from "./Play.vue";

const gameId = ref("");
const playerColor = ref("");
let ws;

async function onCreate() {
    const response = await fetch("http://localhost:3000");
    gameId.value = await response.text();
    console.log("gameId.value:", gameId.value);
    ws = new WebSocket("ws://localhost:3001");
    ws.onopen = () => {
        ws.send(`${gameId.value} join`);
    };
    ws.onmessage = (event) => {
        console.log("message:", event.data);
        playerColor.value = event.data;
    };
}

async function onJoin(gId) {
    console.log("gameId.value:", gameId.value);
    ws = new WebSocket("ws://localhost:3001");
    ws.onopen = () => {
        ws.send(`${gameId.value} join`);
    };
    ws.onmessage = (event) => {
        console.log("message:", event.data);
        playerColor.value = event.data;
    };
    gameId.value = gId;
}
</script>
