<template>
    <div>
        <main>
            <!-- Layout container: left = board, right = sidebar -->
            <section class="board">
                <p>game ID: {{ gameId }}</p>
                <!-- Chessboard area -->
                <the-chessboard
                    @board-created="(api) => (board = api)"
                    @move="handleMove"
                />
            </section>

            <section class="sidebar" aria-label="Game sidebar">
                <!-- Opponent/player labels -->
                <div>
                    <span>they</span>
                </div>
                <div>
                    <span>you</span>
                </div>

                <!-- Move list -->
                <section aria-labelledby="moves-title">
                    <h2 id="moves-title">Moves</h2>
                    <ol>
                        <!-- moves will be listed here -->
                    </ol>
                </section>

                <!-- Actions -->
                <div>
                    <button type="button">offer draw</button>
                    <button type="button">resign</button>
                </div>
            </section>
        </main>
    </div>
</template>

<script setup>
import { onMounted } from "vue";
import { TheChessboard } from "vue3-chessboard";
import "vue3-chessboard/style.css";

let ws;
let board;

const props = defineProps({
    gameId: String,
});

function handleMove({ san }) {
    ws.send(`${props.gameId} move ${san}`);
}

onMounted(() => {
    ws = new WebSocket("ws://localhost:3001");
    ws.onopen = () => {
        ws.send(`${props.gameId} join`);
    };
    ws.onmessage = ({ data }) => {
        console.log(data);
        // TODO: allow resign and draw offering
        board.move(data);
    };
});
</script>

<style scoped>
main {
    display: flex;
}
</style>
