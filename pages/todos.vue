<template>
  <main class="todos">
    <section class="todos__header">
      <button
        class="todos__header-back-button"
        @click="$router.back()"
      >
        Повернутися
      </button>
      <h2 class="todos__header-title">
        {{ user?.name }}
      </h2>
    </section>
    <ul class="todos__list">
      <li
        v-for="todo in todos"
        :key="todo.id"
        class="todos__item"
      >
        <span :class="{ 'todos__item--completed': todo.completed }">
          {{ todo.title }}
        </span>
      </li>
    </ul>
  </main>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import type { Todo, User } from '~/types/types';

const route = useRoute();

const todos = ref<Todo[]>([]);
const user = ref<User>();

const fetchTodos = async () => {
  const userId = route.query.userId;
  const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
  const todosResponse = await fetch(`https://jsonplaceholder.typicode.com/todos?userId=${userId}`);

  user.value = await userResponse.json();
  todos.value = await todosResponse.json();
};

onMounted(() => fetchTodos());
</script>

<style scoped lang="scss">
.todos {
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;

  &__header {
    display: flex;
    align-items: center;
    margin-bottom: 15px;

    &-title {
      font-size: 24px;
      margin-bottom: 15px;
      color: #333;
    }

    &-back-button {
      padding: 8px 10px;
      margin-right: 10px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.3s ease-in;

      &:hover {
        background-color: #0056b3;
      }
    }
  }

  &__list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  &__item {
    padding: 10px;
    border-bottom: 1px solid #ddd;

    &:last-child {
      border-bottom: none;
    }
  }

  &__item--completed {
    text-decoration: line-through;
    color: green;
  }
}
</style>
