<script setup lang="ts">
import { computed } from 'vue';
import nestingIcon from '/workspaces/github-profile/src/assets/Nesting.svg';
import startIcon from '/workspaces/github-profile/src/assets/Star.svg';
import chiledIcon from '/workspaces/github-profile/src/assets/Chield_alt.svg';

const props = defineProps<{ repo: any }>();

const updatedText = computed(() => {
  const d = new Date(props.repo.updated_at);
  const diff = Math.floor((Date.now() - d.getTime()) / 864e5);
  return diff === 0 ? 'updated today' : `updated ${diff} day${diff > 1 ? 's' : ''} ago`;
});
</script>

<template>
  <a
    :href="repo.html_url"
    target="_blank"
    class="block p-5 rounded-xl bg-card-gradient
           hover:-translate-y-1 transition"
  >
    <!-- Tên repo -->
    <h3 class="text-slate text-title mb-4">{{ repo.name }}</h3>

    <!-- Mô tả -->
    <p class="text-small text-slate-light mt-1 line-clamp-2">
      {{ repo.description || 'No description' }}
    </p>

    <!-- Thông tin phụ -->
    <div class="flex items-center flex-wrap gap-4 mt-4 text-body text-slate">
      <!-- License -->
      <span
        v-if="repo.license && repo.license.spdx_id"
        class="flex items-center gap-1"
      >
        <img :src="chiledIcon" alt="license" class="h-6 w-6" />
        {{ repo.license.spdx_id }}
      </span>

      <!-- Forks -->
      <span class="flex items-center gap-1">
        <img :src="nestingIcon" alt="forks" class="h-6 w-6" />
        {{ repo.forks_count }}
      </span>

      <!-- Stars -->
      <span class="flex items-center gap-1">
        <img :src="startIcon" alt="stars" class="h-6 w-6" />
        {{ repo.stargazers_count }}
      </span>

      <!-- Cập nhật -->
      <span class="ml-auto text-slate-light text-small">{{ updatedText }}</span>
    </div>
  </a>
</template>
