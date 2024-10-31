<template>
  <section class="pagination">
    <button
      v-for="page in totalPages"
      :key="page"
      :class="[
        'pagination__button',
        { 'pagination__button--active': page === currentPage },
      ]"
      @click="$emit('change', page)"
    >
      {{ page }}
    </button>
  </section>
</template>

<script setup lang="ts">
defineProps<{
  totalPages: number;
  currentPage: number;
}>();

defineEmits<{
  (e: 'change', page: number): void;
}>();
</script>

<style scoped lang="scss">
.pagination {
  display: flex;
  justify-content: center;
  margin: 20px 0;

  &__button {
    padding: 10px 15px;
    border: 1px solid #007bff;
    background-color: #fff;
    color: #007bff;
    border-radius: 4px;
    cursor: pointer;
    margin: 0 5px;
    transition: background-color 0.3s ease-in, color 0.3s ease-in;

    &:hover {
      background-color: #007bff;
      color: #fff;
    }
  }

  &__button--active {
    background-color: #007bff;
    color: #fff;
  }
}
</style>
