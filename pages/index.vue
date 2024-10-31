<template>
  <SearchBar
    :model-value="searchQuery"
    @update:model-value="searchUsers"
  />
  <UserTable
    :users="users"
    @select="selectUser"
  />
  <Pagination
    v-if="totalPages > 1 && !searchQuery"
    :total-pages="totalPages"
    :current-page="currentPage"
    @change="changePage"
  />
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import SearchBar from '~/components/SearchBar.vue';
import UserTable from '~/components/UserTable.vue';
import Pagination from '~/components/Pagination.vue';
import { useDebounceFn } from '@vueuse/core';
import type { User } from '~/types/types';

const router = useRouter();
const route = useRoute();
const users = ref<User[]>([]);
const currentPage = ref(Number(route.query.page) || 1);
const searchQuery = ref(route.query.search as string || '');
const totalUsers = ref(0);
const usersPerPage = 5;

const totalPages = computed(() => Math.ceil(totalUsers.value / usersPerPage));

const fetchUsers = async (page: number, query = '') => {
  const res = await fetch(`https://jsonplaceholder.typicode.com/users?_page=${page}&_limit=${usersPerPage}&name_like=${query}`);

  users.value = await res.json();
};

const fetchTotalUsers = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');

  totalUsers.value = (await res.json()).length;
};

const changePage = (page: number) => {
  if (page === 1) {
    router.push({ query: {} });
  }

  if (page > 1) {
    router.push({ query: { page } });
  }

  currentPage.value = page;
  fetchUsers(currentPage.value);
};

const searchUsers = useDebounceFn(async (query: string) => {
  searchQuery.value = query.trim();
  currentPage.value = 1;

  router.push({ query: searchQuery.value ? { search: searchQuery.value } : {} });

  await fetchUsers(currentPage.value, searchQuery.value);
}, 300);

const selectUser = (userId: number) => router.push({ name: 'todos', query: { userId } });

onMounted(async () => {
  await fetchUsers(currentPage.value, searchQuery.value);
  await fetchTotalUsers();
});
</script>
