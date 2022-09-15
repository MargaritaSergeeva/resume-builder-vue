<template>
  <div class="alert" v-if="alert" :class="alert.type">
    <h3>{{ alert.title }}</h3>
    <p>{{ alert.text }}</p>
    <app-button :color="alert.type" @action="$emit('close')">Закрыть</app-button>
  </div>
</template>

<script>
import AppButton from '@/AppButton'

export default {
  emits: ['close'],
  props: {
    alert: {
      type: Object,
      default: null,
      validator (value) {
        const hasKeys = Object.keys(value).reduce((has = true, key) => {
          if (!['type', 'text', 'title'].includes(key)) has = false
          if (key === 'type' && !['danger', 'primary'].includes(value[key])) has = false
          if (key === 'text' && typeof value[key] !== 'string') has = false
          if (key === 'title' && typeof value[key] !== 'string') has = false
          return has
        }, [])
        return hasKeys && Object.keys(value).length === 3
      }
    }
  },
  components: {
    AppButton
  }
}
</script>
