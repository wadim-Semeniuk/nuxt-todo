<template>
  <section class="search-bar">
    <input
      :value="modelValue"
      type="text"
      class="search-bar__input"
      placeholder="Пошук користувача"
      @input="onInput"
    />
  </section>
</template>

<script setup lang="ts">
type ExtendedInputEvent = InputEvent & {
  target: {
    value: string,
  }
};

defineProps<{
  modelValue: string;
}>();

const emit = defineEmits<{
 (e: 'update:model-value', value: string): void;
}>();

const onInput = (event: ExtendedInputEvent): void => emit('update:model-value', event.target.value);
</script>

<style lang="scss" scoped>
.search-bar {
  margin-bottom: 20px;

  &__input {
    width: 70%;
    display: block;
    margin: 0 auto;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;

    &:focus {
      border-color: #007bff;
      outline: none;
    }
  }
}
</style>
