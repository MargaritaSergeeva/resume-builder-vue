<template>
  <div class="container">
    <app-alert :alert="alert" @close="alert = null"></app-alert>
  </div>
  <div class="container column">
    <app-form @add="addResumeOption"></app-form>
    <app-loader v-if="isLoading"></app-loader>
    <app-resume-list v-else :options="options" @delete="removeResumeOption"></app-resume-list>
  </div>
  <app-comments-list @error="setAlert"></app-comments-list>
</template>

<script>
import axios from 'axios'
import AppLoader from '@/AppLoader'
import AppAlert from '@/AppAlert'
import AppForm from '@/AppForm'
import AppResumeList from '@/AppResumeList'
import AppCommentsList from '@/AppCommentsList'

export default {
  data () {
    return {
      isLoading: false,
      alert: null,
      options: []
    }
  },
  mounted () {
    this.loadResumeOptions()
  },
  methods: {
    async addResumeOption (form) {
      const { data } = await axios.post('https://resume-builder-vue-default-rtdb.firebaseio.com/resume-list.json', form)
      this.options.push({
        id: data.name,
        ...form
      })
    },
    async loadResumeOptions () {
      this.isLoading = true
      try {
        const { data } = await axios.get('https://resume-builder-vue-default-rtdb.firebaseio.com/resume-list.json')
        if (!data) {
          throw new Error('Список резюме пуст')
        }
        this.options = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
      } catch (e) {
        this.setAlert(e.message)
      } finally {
        this.isLoading = false
      }
    },
    async removeResumeOption (id) {
      try {
        await axios.delete(`https://resume-builder-vue-default-rtdb.firebaseio.com/resume-list/${id}.json`)
        const removedOptionType = this.options.find(option => option.id === id).type
        this.options = this.options.filter(option => option.id !== id)
        this.setAlert(`Опция "${removedOptionType}" была удалена`, 'Успешно!', 'primary')
      } catch (e) {
        this.setAlert(e.message)
      }
    },
    setAlert (message, title = 'Ошибка!', type = 'danger') {
      this.alert = {
        title: title,
        type: type,
        text: message
      }
    }
  },
  components: {
    AppLoader,
    AppAlert,
    AppForm,
    AppResumeList,
    AppCommentsList
  }
}
</script>
