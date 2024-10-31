<template>
  <table
    v-if="users.length"
    class="user-table"
  >
    <thead class="user-table__header">
    <tr>
      <th class="user-table__cell">Ім'я</th>
      <th class="user-table__cell">Email</th>
      <th class="user-table__cell">Телефон</th>
    </tr>
    </thead>
    <tbody class="user-table__body">
    <tr
      v-for="user in users"
      :key="user.id"
      class="user-table__row"
      @click="$emit('select', user.id)"
    >
      <td class="user-table__cell">{{ user.name }}</td>
      <td class="user-table__cell">{{ user.email }}</td>
      <td class="user-table__cell">{{ user.phone }}</td>
    </tr>
    </tbody>
  </table>
  <section
    v-else
    class="no-results"
  >
    No results found
  </section>
</template>

<script setup lang="ts">
import type { User } from '~/types/types';

defineProps<{
  users: User[];
}>();

defineEmits<{
  (e: 'select', id: number): void;
}>();
</script>

<style lang="scss" scoped>
.user-table {
  width: 100%;
  border-radius: 4px;
  border-collapse: collapse;

  &__header {
    background-color: #f9f9f9;
  }

  &__row {
    cursor: pointer;
    transition: background-color 0.3s ease-in;

    &:hover {
      background-color: #f1f1f1;
    }
  }

  &__cell {
    padding: 12px;
    border: 1px solid #ccc;
    text-align: left;
  }
}

.no-results {
  margin: 0 auto;
  font-size: 16px;
  padding: 10px 0;
  width: fit-content;
}
</style>
