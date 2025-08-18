<template>
    <div>
        <main>
            <!-- Layout container: left = board, right = sidebar -->
            <section class="board">
                <p>game ID</p>
                <p class="gameId">{{ gameId }}</p>
                <!-- Chessboard area -->
                <the-chessboard
                    @board-created="(api) => (board = api)"
                    @move="handleMove"
                    :player-color="playerColor"
                    :board-config="boardConfig"
                />
            </section>
        </main>
    </div>
</template>

<script setup>
import { onMounted, ref, computed } from "vue";
import { TheChessboard } from "vue3-chessboard";
import "vue3-chessboard/style.css";

let board;

const props = defineProps({
    gameId: String,
    playerColor: String,
    ws: WebSocket,
});

const boardConfig = {
    orientation: props.playerColor,
    position: "start",
    draggable: {
        enabled: true,
    },
};

function handleMove({ color, san }) {
    const formattedPlayerColor = props.playerColor == "white" ? "w" : "b";
    if (formattedPlayerColor == color) {
        props.ws.send(`${props.gameId} move ${san}`);
    }
}

onMounted(() => {
    props.ws.onmessage = ({ data }) => {
        console.log("move:", data);
        board.move(data);
    };
});
</script>

<style scoped>
main {
    display: flex;
    user-select: none;
}
.gameId {
    /* TODO: De-hardcode colors */
    user-select: text;
    background-color: #f0f0f0;
    color: #333;
    width: fit-content;
    margin: auto;
    margin-bottom: 1rem;
}
</style>
