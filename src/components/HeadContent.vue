<script setup>
import { ref } from "vue";
import searchIcon from "/workspaces/github-profile/src/assets/Search.svg";

const emit = defineEmits(["search"]);
const inputValue = ref("");
function handleKeyPress(e) {
  if (e.key === "Enter" && inputValue.value.trim()) {
    emit("search", inputValue.value.trim());
  }
}
</script>

<template>
  <div class="relative">
    <!-- Banner -->
    <img
      class="w-full object-cover"
      src="/workspaces/github-profile/src/assets/hero-image-github-profile-sm.jpg"
      alt="Banner Profile"
    />
    <div class="absolute top-4 left-1/2 -translate-x-1/2">
      <div
        class="flex items-center w-md max-w-full px-4 py-3 rounded-xl bg-app focus-within:ring-2 focus-within:ring-blue-500"
      >
        <!-- Icon -->
        <img :src="searchIcon" alt="search" />
        <!-- Input -->
        <input
          type="search"
          v-model="inputValue"
          @keypress="handleKeyPress"
          placeholder="Tìm kiếm..."
          class="flex-1 bg-transparent text-slate text-body focus:outline-none ml-2"
        />
      </div>
    </div>
  </div>
</template>
<style scoped></style>
