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
        <option value="subscriber">Subscribed</option>
        <option value="unsubscriber">Unsubscribed</option>
      </select>
    </div>

    <div v-if="error">{{ error }}</div>

    <button type="submit">Add Subscriber</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const API_BASE = import.meta.env.VITE_API_BASE_URL


const form = ref({
  email: '',
  status: '',
})

const error = ref(null)

const submitForm = async () => {
  error.value = null

  try {
    await axios.post(`${API_BASE}/subscribers`, form.value)
    form.value.email = ''
    form.value.status = ''
    alert('Subscriber added successfully!')
  } catch (err) {
    error.value = err.response?.data?.message || 'Something went wrong.'
  }
}
</script>
