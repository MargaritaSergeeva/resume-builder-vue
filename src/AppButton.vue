<template>
  <button class="btn" :class="color" :type="type" @click="$emit('action')">
    <slot />
  </button>
</template>

<script>
export default {
  props: {
    emits: {
      action: null
    },
    color: {
      type: String,
      default: '',
      validator (value) {
        return ['', 'danger', 'primary'].includes(value)
      }
    },
    type: {
      type: String,
      default: 'button'
    }
  }
}
</script>
