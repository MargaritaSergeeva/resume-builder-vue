<template>
  <div class="card card-w70">
    <div v-if="options.length">
      <component
        v-for="(option, index) in options"
        :key="index"
        :is="`app-${option.type}`"
        v-bind="option"
        @delete="$emit('delete', $event)"
      >
      </component>
    </div>
    <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
  </div>
</template>

<script>
import AppTitle from '@/AppTitle'
import AppSubtitle from '@/AppSubtitle'
import AppText from '@/AppText'
import AppAvatar from '@/AppAvatar'

export default {
  emits: {
    delete: payload => {
      return typeof payload === 'string'
    }
  },
  props: {
    options: {
      type: Array,
      required: true,
      validator (values) {
        const arr = values.map(value => {
          const hasKeys = Object.keys(value).reduce((has, key) => {
            if (!['type', 'text', 'id'].includes(key)) has = false
            if (key === 'type' && !['title', 'subtitle', 'avatar', 'text'].includes(value[key])) has = false
            if (key === 'text' && typeof value[key] !== 'string') has = false
            if (key === 'id' && typeof value[key] !== 'string') has = false
            return has
          }, true)
          return hasKeys && Object.keys(value).length === 3
        })
        return !arr.includes(false)
      }
    }
  },
  components: {
    AppTitle,
    AppSubtitle,
    AppText,
    AppAvatar
  }
}
</script>
