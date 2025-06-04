<script setup>
import { ref } from 'vue'

// State Form
const nama = ref('')
const umur = ref('')
const kategori = ref('')
const peserta = ref([])

const error = ref('')
const successMessage = ref('')

// Fungsi validasi dan pendaftaran
const daftarPeserta = () => {
  if (!nama.value || !umur.value || !kategori.value) {
    error.value = 'Semua kolom wajib diisi!'
    successMessage.value = ''
    return
  }

  if (umur.value < 10) {
    error.value = 'Umur minimal 10 tahun.'
    successMessage.value = ''
    return
  }

  peserta.value.push({
    nama: nama.value.trim(),
    umur: umur.value,
    kategori: kategori.value,
  })

  // Reset form dan tampilkan pesan sukses
  nama.value = ''
  umur.value = ''
  kategori.value = ''
  error.value = ''
  successMessage.value = 'Pendaftaran berhasil!'
  
  setTimeout(() => {
    successMessage.value = ''
  }, 3000)
}
</script>

<template>
  <div class="container">
    <h1>Pendaftaran Turnamen Badminton</h1>
    <form @submit.prevent="daftarPeserta" class="form-box">
      <div class="form-group">
        <label for="nama">Nama:</label>
        <input
          v-model="nama"
          type="text"
          id="nama"
          placeholder="Contoh: Fhelia Zahwa"
        />
      </div>

      <div class="form-group">
        <label for="umur">Umur:</label>
        <input
          v-model="umur"
          type="number"
          id="umur"
          min="10"
          placeholder="Minimal 10 tahun"
        />
      </div>

      <div class="form-group">
        <label for="kategori">Kategori:</label>
        <select v-model="kategori" id="kategori">
          <option disabled value="">-- Pilih Kategori --</option>
          <option value="Junior">Junior (≤17 tahun)</option>
          <option value="Senior">Senior (>17 tahun)</option>
        </select>
      </div>

      <button type="submit" :disabled="!nama || !umur || !kategori">
        Daftar Sekarang
      </button>

      <!-- Conditional Rendering -->
      <p v-if="error" class="error">{{ error }}</p>
      <p v-if="successMessage" class="success">{{ successMessage }}</p>
    </form>

    <div v-if="peserta.length > 0" class="list-box">
      <h2>📋 Daftar Peserta</h2>
      <ul>
        <li v-for="(p, index) in peserta" :key="index">
          {{ index + 1 }}. {{ p.nama }} ({{ p.umur }} tahun) - Kategori: {{ p.kategori }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 30px auto;
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #2c3e50;
}

.form-box {
  background: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 6px;
}

input,
select {
  width: 100%;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid #ccc;
  font-size: 14px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #42b983;
  color: white;
  font-weight: bold;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s;
}

button:disabled {
  background-color: #aaa;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #36976b;
}

.error {
  color: #e74c3c;
  margin-top: 10px;
}

.success {
  color: #2ecc71;
  margin-top: 10px;
}

.list-box {
  margin-top: 30px;
  background: #fff;
  border: 1px solid #ddd;
  padding: 15px;
  border-radius: 10px;
}

ul {
  list-style-type: decimal;
  padding-left: 20px;
}
</style>
