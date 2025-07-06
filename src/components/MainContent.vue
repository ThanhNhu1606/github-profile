<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import RepoCard from "./RepoCard.vue";

const props = defineProps<{ username: string }>();

const user = ref<any | null>(null);
const repos = ref<any[]>([]);
const busy = ref(false);
const error = ref<string | null>(null);

async function loadData() {
  if (!props.username) return;
  busy.value = true;
  error.value = null;

  try {
    const userRes = await fetch(`https://api.github.com/users/${props.username}`);
    if (!userRes.ok) throw new Error("Không tìm thấy người dùng");
    user.value = await userRes.json();

    const repoRes = await fetch(
      `https://api.github.com/users/${props.username}/repos?sort=updated&per_page=4`
    );
    repos.value = await repoRes.json();
  } catch (e: any) {
    error.value = e.message ?? "Đã có lỗi xảy ra";
  } finally {
    busy.value = false;
  }
}

onMounted(loadData);
watch(() => props.username, loadData);
</script>

<template>
  <!-- Lỗi -->
  <p v-if="error" class="text-center text-title text-slate py-10">{{ error }}</p>

  <!-- Đang tải -->
  <div v-else-if="busy" class="flex justify-center py-20">
    <span class="h-10 w-10 border-4 border-white/20 border-t-white rounded-full animate-spin" />
  </div>

  <!-- Nội dung -->
  <section v-else-if="user" class="max-w-5xl mx-auto px-6 lg:px-0 text-body text-slate pb-16">
    <!-- Avatar + Thống kê -->
    <div class="flex flex-col items-start gap-4 md:gap-6">
      <!-- Khối avatar + thống kê -->
      <div class="flex items-center gap-6">
        <!-- Avatar -->
        <img
          :src="user.avatar_url"
          alt="avatar"
          class="w-32 h-32 rounded-3xl -mt-12 relative z-20 border-8 border border-[#20293A]"
        />

        <!-- Thống kê -->
        <div class="flex flex-wrap gap-4 text-body text-slate">
          <!-- Followers -->
          <div class="flex bg-card-dark px-4 py-4 rounded-xl divide-x divide-gray-600">
            <span class="pr-3">Followers</span>
            <span class="pl-3  ">{{ user.followers }}</span>
          </div>

          <!-- Following -->
          <div class="flex bg-card-dark px-4 py-4 rounded-xl divide-x divide-gray-600">
            <span class="pr-3">Following</span>
            <span class="pl-3">{{ user.following }}</span>
          </div>

          <!-- Location -->
          <div class="flex bg-card-dark px-4 py-4 rounded-xl divide-x divide-gray-600">
            <span class="pr-3">Location</span>
            <span class="pl-3">{{ user.location || "N/A" }}</span>
          </div>
        </div>
      </div>

      <!-- Tên + Bio -->
      <div>
        <h1 class="text-title text-large text-slate">
          {{ user.name || user.login }}
        </h1>
        <p class="text-body mt-1">{{ user.bio }}</p>
      </div>
    </div>

    <!-- Danh sách repo -->
    <div class="grid gap-6 md:grid-cols-2 mt-10 mb-4">
      <RepoCard v-for="repo in repos" :key="repo.id" :repo="repo" />
    </div>

    <!-- Xem thêm -->
    <a
      v-if="user.public_repos > 8"
      :href="`https://github.com/${user.login}?tab=repositories`"
      target="_blank"
      class="block text-center mt-12 text-slate text-body"
    >
      View all repositories
    </a>
  </section>
</template>
