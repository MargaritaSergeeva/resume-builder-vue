<template>
  <div class="row">
    <h2>{{ $attrs.text }}</h2>
    <app-button view="cross" @action="$emit('delete', $attrs.id)"></app-button>
  </div>
</template>

<script>
import AppButton from '@/AppButton'

export default {
  emits: {
    delete: payload => {
      return typeof payload === 'string'
    }
  },
  components: {
    AppButton
  }
}
</script>
