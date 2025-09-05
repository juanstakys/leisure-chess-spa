<template>
    <div>
        <main>
            <!-- Layout container: left = board, right = sidebar -->
            <!-- TODO: add copy button besides -->
            <p>
                <span>game ID: </span>
                <span class="gameId">{{ gameId }}</span>
                <a>click to copy</a>
            </p>
            <!-- Chessboard area -->
            <section class="board">
                <the-chessboard
                    @board-created="(api) => (board = api)"
                    @move="handleMove"
                    :player-color="playerColor"
                    :board-config="boardConfig"
                />
                <dialog>
                    <button autofocus>Close</button>
                    <p>{{ gameOverMsg }}</p>
                </dialog>
            </section>
        </main>
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import { TheChessboard } from "vue3-chessboard";
import "vue3-chessboard/style.css";

let board;

const gameOverMsg = ref("");

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
        if (data.startsWith("Game over:")) {
            gameOverMsg.value = data;
            dialog.showModal();
            return;
        }
        board.move(data);
    };
    const dialog = document.querySelector("dialog");
    const showButton = document.querySelector("dialog + button");
    const closeButton = document.querySelector("dialog button");

    // "Close" button closes the dialog
    closeButton.addEventListener("click", () => {
        dialog.close();
    });
});
</script>

<style scoped>
main {
    user-select: none;
}
.gameId {
    /* TODO: De-hardcode colors */
    user-select: text;
    background-color: #f0f0f0;
    color: #333;
    width: fit-content;
    margin: 0.5rem;
    padding: 0.2rem;
    border-radius: 0.2rem;
}
.board {
    margin-bottom: 1rem;
    width: 90vw;
}
::backdrop {
    background-color: black;
    opacity: 0.75;
}
</style>
