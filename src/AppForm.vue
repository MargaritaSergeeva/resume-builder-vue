<template>
  <form class="card card-w30" @submit.prevent="sendForm">
    <div class="form-control">
      <label for="type">Тип блока</label>
      <select v-model="form.type" id="type" name="type">
        <option value="title">Заголовок</option>
        <option value="subtitle">Подзаголовок</option>
        <option value="avatar">Аватар</option>
        <option value="text">Текст</option>
      </select>
    </div>

    <div class="form-control">
      <label for="value">Значение</label>
      <textarea v-model="form.text" name="text" id="value" rows="3"></textarea>
    </div>

    <app-button color="primary" type="submit" :is-disabled="isDisabled">Добавить</app-button>
  </form>
</template>

<script>
import AppButton from '@/AppButton'

export default {
  emits: {
    add: payload => {
      let hasKeys = false
      if (typeof payload === 'object') {
        hasKeys = Object.keys(payload).reduce((has = true, key) => {
          if (!['type', 'text'].includes(key)) has = false
          if (key === 'type' && !['title', 'subtitle', 'avatar', 'text'].includes(payload[key])) has = false
          if (key === 'text' && typeof payload[key] !== 'string') has = false
          return has
        }, [])
      }
      return hasKeys && Object.keys(payload).length === 2
    }
  },
  data () {
    return {
      form: {
        type: 'title',
        text: ''
      }
    }
  },
  methods: {
    sendForm () {
      if (this.form.type && this.form.text) {
        this.$emit('add', { ...this.form })
        this.form.type = 'title'
        this.form.text = ''
      }
    }
  },
  computed: {
    isDisabled () {
      return this.form.text.length <= 3
    }
  },
  components: {
    AppButton
  }
}
</script>
