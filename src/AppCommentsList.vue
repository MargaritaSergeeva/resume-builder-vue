<template>
  <div class="container">
    <p>
      <app-button color="primary" @action="loadComments">Загрузить комментарии</app-button>
    </p>

    <app-loader v-if="isLoading"></app-loader>

    <div v-if="isShowedComments">
      <div v-if="!isLoading" class="card">
        <h2>Комментарии</h2>
        <ul class="list">
          <li v-for="comment in this.comments" :key="comment.id" class="list-item">
            <div>
              <p><strong>{{ comment.email }}</strong></p>
              <small>{{ comment.text }}</small>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import AppButton from '@/AppButton'
import AppLoader from '@/AppLoader'

export default {
  emits: {
    error: payload => {
      return typeof payload === 'string'
    }
  },
  data () {
    return {
      isLoading: false,
      isShowedComments: false,
      comments: []
    }
  },
  methods: {
    async loadComments () {
      this.isLoading = true
      try {
        const { data } = await axios.get('https://jsonplaceholder.typicode.com/comments?_limit=42')
        if (!data.length) {
          throw new Error('Нет комментариев')
        }
        this.comments = data.map(item => {
          return {
            id: item.id,
            email: item.email,
            text: item.body
          }
        })
        this.isShowedComments = true
      } catch (e) {
        this.$emit('error', e.message)
      } finally {
        this.isLoading = false
      }
    }
  },
  components: {
    AppButton,
    AppLoader
  }
}
</script>
