<template>
  <button class="buttonContent" @click="handleClick">
    <slot>
      {{ title }}
    </slot>
  </button>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'CustomButton',
  props: {
    title: String,
    onClick: Function
  },
  setup(props) {
    const handleClick = () => {
      props.onClick()
    }
    return { handleClick }
  }
})
</script>

<style>
.buttonContent {
  cursor: pointer;
  background-color: #d3e4f6;
  border-radius: 15px;
  padding: 0.7rem;
  border: 1px solid lightgray;
  font-weight: 600;
  box-shadow: 2px 1px 3px 0px lightgray;
}
</style>
