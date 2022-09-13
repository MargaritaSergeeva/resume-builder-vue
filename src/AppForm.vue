<template>
  <form class="card card-w30" @submit.prevent="$emit('addResumeOption', {...form})">
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

    <app-button color="primary" type="submit">Добавить</app-button>
  </form>
</template>

<script>
import AppButton from '@/AppButton'

export default {
  emits: ['add-resume-option'],
  data () {
    return {
      form: {
        type: 'title',
        text: ''
      }
    }
  },
  components: {
    AppButton
  }
}
</script>
