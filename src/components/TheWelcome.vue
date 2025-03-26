<template>
  <form @submit.prevent="submitForm">
    <div>
      <label>Email</label>
      <input type="email" v-model="form.email" required />
    </div>

    <div>
      <label>Status</label>
      <select v-model="form.status" required>
        <option value="">Select status</option>
        <option value="subscribed">Subscribed</option>
        <option value="unsubscribed">Unsubscribed</option>
      </select>
    </div>

    <div v-if="error">{{ error }}</div>

    <button type="submit">Add Subscriber</button>
  </form>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      form: {
        email: '',
        status: '',
      },
      error: null,
      API_BASE: import.meta.env.VITE_API_BASE_URL,
    }
  },
  methods: {
    async submitForm() {
      this.error = null
      try {
        await axios.post(`${this.API_BASE}/subscribers`, this.form)
        this.form.email = ''
        this.form.status = ''
        alert('Subscriber added successfully!')
      } catch (err) {
        this.error = err.response?.data?.message || 'Something went wrong.'
      }
    },
  },
}
</script>
