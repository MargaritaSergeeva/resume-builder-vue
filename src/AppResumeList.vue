<template>
  <div class="card card-w70">
    <div v-if="options.length">
      <component v-for="(option, index) in options" :key="index" :is="`app-${option.type}`" v-bind="option"></component>
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
  props: {
    options: {
      type: Array,
      required: true
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

<style scoped>

</style>
