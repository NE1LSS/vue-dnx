
<script setup>
import { onMounted, ref } from 'vue'
import api from './services/api'

const users = ref([])
const isLoading = ref(true)
const errorMessage = ref('')

async function loadUsers() {
  try {
    const response = await api.get('/api/api/users')
    users.value = response.data.data
  } catch (error) {
    errorMessage.value = error.message || 'Gagal mengambil data pengguna.'
  } finally {
    isLoading.value = false
  }
}

onMounted(loadUsers)
</script>

<template>
  <main class="container">
    <h1>Daftar Pengguna</h1>

    <p v-if="isLoading">Memuat data...</p>

    <p v-else-if="errorMessage" class="error">
      {{ errorMessage }}
    </p>

    <p v-else-if="users.length === 0">
      Data pengguna tidak ditemukan.
    </p>

    <div v-else class="user-list">
      <div v-for="user in users" :key="user.id" class="user-card">
        <h2>{{ user.name }}</h2>
        <p>{{ user.email }}</p>
      </div>
    </div>
  </main>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f3f4f6;
  color: #1f2937;
  font-family: Arial, sans-serif;
}

.container {
  width: min(800px, calc(100% - 32px));
  margin: 40px auto;
}

.user-list {
  display: grid;
  gap: 16px;
}

.user-card {
  padding: 16px;
  border-radius: 8px;
  background: white;
  box-shadow: 0 2px 8px #00000014;
}

.error {
  color: #b91c1c;
}
</style>
