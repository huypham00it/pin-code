<template>
  <div>
    <HSelect :options="options" v-model="selectedValue" />
  </div>
</template>

<script>
import { mount } from '@vue/test-utils'
import HSelect from '../src/components/h-select/HSelect.vue'

describe('HSelect', () => {
  const options = [
    { label: 'Option 1', value: 'option1' },
    { label: 'Option 2', value: 'option2' },
    { label: 'Option 3', value: 'option3' }
  ]

  it('renders the select options correctly', () => {
    const wrapper = mount(HSelect, {
      props: {
        options
      }
    })

    const selectOptions = wrapper.findAll('option')

    expect(selectOptions.length).toBe(options.length)

    selectOptions.forEach((option, index) => {
      expect(option.text()).toBe(options[index].label)
      expect(option.attributes('value')).toBe(options[index].value)
    })
  })

  it('updates the selected value when an option is selected', async () => {
    const wrapper = mount(HSelect, {
      props: {
        options
      }
    })

    const select = wrapper.find('select')

    await select.setValue(options[1].value)

    expect(wrapper.vm.selectedValue).toBe(options[1].value)
  })

  it('emits the updated selected value when an option is selected', async () => {
    const wrapper = mount(HSelect, {
      props: {
        options
      }
    })

    const select = wrapper.find('select')

    await select.setValue(options[2].value)

    expect(wrapper.emitted('update:modelValue')).toBeTruthy()
    expect(wrapper.emitted('update:modelValue')[0][0]).toBe(options[2].value)
  })
})

export default {
  components: {
    HSelect
  },
  data() {
    return {
      options: [
        { label: 'Option 1', value: 'option1' },
        { label: 'Option 2', value: 'option2' },
        { label: 'Option 3', value: 'option3' }
      ],
      selectedValue: ''
    }
  }
}
</script>
