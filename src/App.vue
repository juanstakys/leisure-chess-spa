<template>
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

async function onCreate(color) {
    console.log(color);
    const response = await fetch(
        `http://${import.meta.env.VITE_SERVER_IP}:3000/${color}`,
    );
    gameId.value = await response.text();
    console.log("gameId.value:", gameId.value);
    ws = new WebSocket(`ws://${import.meta.env.VITE_SERVER_IP}:3001`);
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
    ws = new WebSocket(`ws://${import.meta.env.VITE_SERVER_IP}:3001`);
    ws.onopen = () => {
        ws.send(`${gameId.value} join`);
    };
    ws.onmessage = (event) => {
        console.log("message:", event.data);
        playerColor.value = event.data;
        // TODO: Hande game not found
    };
    gameId.value = gId;
}
</script>
