<template>
  <div>
    <HButton @click="handleClick">Click me</HButton>
    <p v-if="isClicked">Button clicked!</p>
  </div>
</template>

<script>
import { mount } from '@vue/test-utils'
import HButton from '../src/components/h-button/HButton.vue'

describe('HButton', () => {
  it('emits click event when clicked', async () => {
    const wrapper = mount(HButton)
    const button = wrapper.find('button')

    await button.trigger('click')

    expect(wrapper.emitted('click')).toBeTruthy()
  })

  it('disables the button when disabled prop is true', () => {
    const wrapper = mount(HButton, {
      props: {
        disabled: true
      }
    })

    expect(wrapper.find('button').attributes('disabled')).toBeTruthy()
  })

  it('applies the correct button type', () => {
    const wrapper = mount(HButton, {
      props: {
        type: 'submit'
      }
    })

    expect(wrapper.find('button').attributes('type')).toBe('submit')
  })

  it('renders the slot content', () => {
    const wrapper = mount(HButton, {
      slots: {
        default: 'Custom Button'
      }
    })

    expect(wrapper.text()).toContain('Custom Button')
  })

  it('renders the button with the correct styles', () => {
    const wrapper = mount(HButton)

    expect(wrapper.find('button').classes()).toContain('p-4')
    expect(wrapper.find('button').classes()).toContain('bg-[#A0C1FF]')
    expect(wrapper.find('button').classes()).toContain('rounded-2xl')
    expect(wrapper.find('button').classes()).toContain('text-base')
    expect(wrapper.find('button').classes()).toContain('font-semibold')
    expect(wrapper.find('button').classes()).toContain('text-white')
    expect(wrapper.find('button').classes()).toContain('max-w-sm')
    expect(wrapper.find('button').classes()).toContain('mx-auto')
    expect(wrapper.find('button').classes()).toContain('w-full')
  })
})

export default {
  components: {
    HButton
  },
  data() {
    return {
      isClicked: false
    }
  },
  methods: {
    handleClick() {
      this.isClicked = true
    }
  }
}
</script>
