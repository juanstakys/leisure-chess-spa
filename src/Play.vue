<script setup>
import { TheChessboard } from "vue3-chessboard";

import "vue3-chessboard/style.css";
</script>

<template>
    <div>
        <main>
            <!-- Layout container: left = board, right = sidebar -->
            <section class="board">
                <!-- Chessboard area -->
                <the-chessboard />
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

<style scoped>
main {
    display: flex;
}
</style>
