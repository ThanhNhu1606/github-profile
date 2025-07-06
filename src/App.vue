<script setup>
import { ref } from 'vue';
import HeadContent from './components/HeadContent.vue';
import MainContent from './components/MainContent.vue';

const username = ref('');
</script>

<template >
<div class="bg-app min-h-screen">
    <HeadContent @search="username = $event" />
    <MainContent v-if="username" :username="username" />
  </div>
</template>

<style scoped>

</style>
