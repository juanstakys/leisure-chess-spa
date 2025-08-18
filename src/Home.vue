<template>
    <main>
        <section role="region" aria-label="Chess Now landing">
            <form @submit.prevent="onJoin">
                <label for="gameId">join:</label>
                <div>
                    <input
                        id="gameId"
                        v-model.trim="gameId"
                        type="text"
                        placeholder="Enter Game ID"
                        autocomplete="off"
                        spellcheck="false"
                        autofocus
                        @keyup.enter="onJoin"
                    />
                    <button
                        type="submit"
                        :disabled="!canJoin"
                        aria-label="Join game"
                        title="Join game"
                    >
                        <span aria-hidden="true">→</span>
                    </button>
                </div>
            </form>

            <div aria-hidden="true">or</div>

            <ColorModal @color-selected="createGame" />
        </section>
    </main>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import ColorModal from "./ColorModal.vue";

const gameId = ref("");

const canJoin = computed(() => gameId.value.trim().length > 0);

const emit = defineEmits<{
    (e: "join", id: string): void;
    (e: "create"): void;
}>();

function onJoin() {
    if (!canJoin.value) return;
    emit("join", gameId.value.trim());
}

function createGame(color) {
    emit("create", color);
}
</script>
