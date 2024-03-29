<script setup lang="ts">
import PinInput from '@/components/pin-input/PinInput.vue'
import HButton from '@/components/h-button/HButton.vue'
import HSelect from '@/components/h-select/HSelect.vue'

import { ref } from 'vue'

const model = ref('')
const disabled = ref(true)
const selectedValue = ref(6)
const secureMode = ref(0)

const handleCompleted = (val: string) => {
  model.value = val
  disabled.value = false
}

const handleSubmitted = () => {
  alert('Submitted:' + model.value)
  disabled.value = true
}
</script>

<template>
  <main class="flex-1 flex flex-col justify-center items-center bg-[#E7ECF2] w-full h-full">
    <section class="flex gap-4">
      <div>
        <label for="code-length">Dynamic length</label>
        <HSelect
          id="code-length"
          :options="[
            { label: 'Length 4', value: 4 },
            { label: 'Length 6', value: 6 }
          ]"
          v-model="selectedValue"
        />
      </div>
      <div>
        <label for="code-length">Switch mode</label>
        <HSelect
          id="code-length"
          :options="[
            { label: 'Secret', value: 1 },
            { label: 'Default', value: 0 }
          ]"
          v-model="secureMode"
        />
      </div>
    </section>

    <section class="bg-white rounded-3xl p-10 shadow-lg flex flex-col gap-8">
      <h1 class="text-center text-3xl font-bold mb-4">PIN CODE</h1>
      <PinInput
        :secure="secureMode == 1 ? true : false"
        v-model="model"
        :length="Number(selectedValue)"
        autofocus
        input-class="pinInput"
        @completed="handleCompleted"
      />

      <HButton :disabled="disabled" @click="handleSubmitted"> Submit </HButton>
    </section>
  </main>
</template>
