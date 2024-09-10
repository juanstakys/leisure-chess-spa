<script setup>
import LoginButton from "./components/LoginButton.vue";
import LogoutButton from "./components/LogoutButton.vue";
import { TheChessboard } from "vue3-chessboard";
import "vue3-chessboard/style.css";
import { useAuth0 } from "@auth0/auth0-vue";

const { user, isAuthenticated } = useAuth0();
</script>

<template>
    <div v-if="isAuthenticated">
      <img :src="user.picture" alt="User picture">
      <p>{{ user.name }}</p>
      <logout-button />
    </div>
    <login-button v-else />
    <the-chessboard />
</template>

<style scoped></style>
