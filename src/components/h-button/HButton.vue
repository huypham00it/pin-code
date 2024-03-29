<template>
  <button
    :type="type"
    :class="[
      'p-4 bg-[#A0C1FF] rounded-2xl text-base font-semibold text-white max-w-sm mx-auto w-full',
      { disabled: disabled }
    ]"
    @click="$emit('click')"
  >
    <slot />
  </button>
</template>

<script lang="ts" setup>
import { withDefaults } from 'vue'
import type { ButtonProps } from './HButton.types'

const props = withDefaults(defineProps<ButtonProps>(), {
  disabled: false,
  type: 'button'
})

const emits = defineEmits(['click'])
</script>

<style scoped>
button {
  padding: 16px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button.disabled {
  opacity: 0.5;
  cursor: default;
}
</style>
