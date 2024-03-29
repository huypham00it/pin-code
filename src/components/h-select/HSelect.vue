<template>
  <div class="h-select">
    <select v-model="selectedValue" @change="handleChange">
      <option v-for="option in options" :key="option.value" :value="option.value">
        {{ option.label }}
      </option>
    </select>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import type { Option } from './HSelect.types'

export default defineComponent({
  name: 'HSelect',
  props: {
    options: {
      type: Array as () => Option[],
      required: true
    },
    value: {
      type: [String, Number],
      default: ''
    }
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const selectedValue = ref(props.value)

    const handleChange = (event: Event) => {
      const target = event.target as HTMLSelectElement
      const value = target.value
      selectedValue.value = value
      emit('update:modelValue', value)
    }

    return {
      selectedValue,
      handleChange
    }
  }
})
</script>

<style scoped></style>
