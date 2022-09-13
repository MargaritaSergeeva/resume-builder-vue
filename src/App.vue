<template>
  <div class="container">
    <app-alert :alert="alert" @close="alert = null"></app-alert>
  </div>
  <div class="container column">
    <app-form @add-resume-option="addResumeOption"></app-form>
    <app-resume-list :options="options"></app-resume-list>
  </div>
  <app-comments-list @error="setErrorAlert"></app-comments-list>
</template>

<script>
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
  methods: {
    addResumeOption (form) {
      this.options.push(form)
      console.log(this.options)
    },
    setErrorAlert (message) {
      this.alert = {
        title: 'Ошибка!',
        type: 'danger',
        text: message
      }
    }
  },
  components: {
    AppAlert,
    AppForm,
    AppResumeList,
    AppCommentsList
  }
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
