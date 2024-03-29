<template>
  <div>
    <PinInput v-model="pinCode" />
  </div>
</template>

<script>
import { mount } from '@vue/test-utils'
import PinInput from './PinInput.vue'

describe('PinInput', () => {
  it('renders the correct number of input cells', () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    expect(inputCells.length).toBe(wrapper.vm.$props.length)
  })

  it('updates the model value when input cells are filled', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    for (let i = 0; i < inputCells.length; i++) {
      await inputCells[i].setValue('1')
    }

    expect(wrapper.vm.$props.modelValue).toBe('1111')
  })

  it('emits the "completed" event when all input cells are filled', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    for (let i = 0; i < inputCells.length; i++) {
      await inputCells[i].setValue('1')
    }

    expect(wrapper.emitted('completed')).toBeTruthy()
    expect(wrapper.emitted('completed')[0][0]).toBe('1111')
  })

  it('blurs the last input cell when "blurOnComplete" prop is true', async () => {
    const wrapper = mount(PinInput, {
      props: {
        blurOnComplete: true
      }
    })
    const inputCells = wrapper.findAll('input')

    for (let i = 0; i < inputCells.length; i++) {
      await inputCells[i].setValue('1')
    }

    expect(inputCells[inputCells.length - 1].element).not.toBe(document.activeElement)
  })

  it('focuses the next input cell when the right arrow key is pressed', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    await inputCells[0].trigger('keydown', { keyCode: 39 })

    expect(inputCells[1].element).toBe(document.activeElement)
  })

  it('focuses the previous input cell when the left arrow key is pressed', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    await inputCells[1].trigger('keydown', { keyCode: 37 })

    expect(inputCells[0].element).toBe(document.activeElement)
  })

  it('erases the value of the previous input cell when the delete key is pressed', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    await inputCells[1].setValue('1')
    await inputCells[1].trigger('keydown.delete')

    expect(inputCells[0].element.value).toBe('')
  })

  it('pastes the values into the input cells when the paste event is triggered', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    await inputCells[0].trigger('paste', {
      clipboardData: {
        getData: () => '1234'
      }
    })

    expect(inputCells[0].element.value).toBe('1')
    expect(inputCells[1].element.value).toBe('2')
    expect(inputCells[2].element.value).toBe('3')
    expect(inputCells[3].element.value).toBe('4')
  })

  it('updates the model value when input cells are filled via input event', async () => {
    const wrapper = mount(PinInput)
    const inputCells = wrapper.findAll('input')

    await inputCells[0].trigger('input', { data: '1' })
    await inputCells[1].trigger('input', { data: '2' })
    await inputCells[2].trigger('input', { data: '3' })
    await inputCells[3].trigger('input', { data: '4' })

    expect(wrapper.vm.$props.modelValue).toBe('1234')
  })
})
</script>
