<script setup>
import { ref } from 'vue'

// Declarative Rendering: menu yang akan dirender di template
const menu = ref([
  { id: 1, nama: 'Nasi Goreng', harga: 20000, tersedia: true },
  { id: 2, nama: 'Mie Ayam', harga: 18000, tersedia: true },
  { id: 3, nama: 'Ayam Bakar', harga: 25000, tersedia: false },
  { id: 4, nama: 'Es Teh Manis', harga: 5000, tersedia: true },
])

// Form Bindings: variabel yang diikat ke input form
const itemDipilih = ref(null)
const nama = ref('')
const meja = ref('')
const catatan = ref('')
const pesanTerkirim = ref(false)

// Event Listeners: fungsi saat item dipilih
const pilihItem = (item) => {
  itemDipilih.value = item
  pesanTerkirim.value = false
}

// Event Listeners: fungsi saat form dikirim
const kirimPesanan = () => {
  if (nama.value && meja.value && itemDipilih.value) {
    console.log(`Pesanan dikirim oleh ${nama.value} (Meja ${meja.value}): ${itemDipilih.value.nama}`)
    pesanTerkirim.value = true

    // Reset form
    nama.value = ''
    meja.value = ''
    catatan.value = ''
    itemDipilih.value = null
  }
}
</script>

<template>
  <h1>Menu Makanan</h1>

  <!-- Declarative Rendering: daftar menu dengan v-for -->
  <ul>
    <li v-for="item in menu" :key="item.id">
      {{ item.nama }} - Rp{{ item.harga }}
      <span>({{ item.tersedia ? 'Tersedia' : 'Habis' }})</span>

      <!-- Attribute Bindings: bind ke disabled dan class -->
      <!-- Event Listeners: tombol klik -->
      <button
        :disabled="!item.tersedia"
        :class="item.tersedia ? 'btn-aktif' : 'btn-nonaktif'"
        @click="pilihItem(item)"
      >
        Pilih
      </button>
    </li>
  </ul>

  <!-- Conditional Rendering: tampilkan form hanya jika itemDipilih tidak null -->
  <div v-if="itemDipilih">
    <form @submit.prevent="kirimPesanan">
      <h2>Formulir Pemesanan</h2>

      <p>Anda memilih: <strong>{{ itemDipilih.nama }}</strong></p>

      <!-- Form Bindings: input nama -->
      <label for="nama">Nama:</label>
      <input type="text" id="nama" v-model="nama" placeholder="Nama Pemesan" required />

      <!-- Form Bindings: input nomor meja -->
      <label for="meja">No Meja:</label>
      <input type="number" id="meja" v-model="meja" placeholder="Contoh: 12" required />

      <!-- Form Bindings: textarea catatan -->
      <label for="catatan">Catatan:</label>
      <textarea id="catatan" v-model="catatan" placeholder="Tambahan seperti 'tidak pedas', dll"></textarea>

      <!-- Event Listeners: tombol kirim form -->
      <button type="submit" class="btn-aktif">Kirim Pesanan</button>
    </form>
  </div>

  <!-- Conditional Rendering: pesan sukses setelah submit -->
  <div v-if="pesanTerkirim">
    <p>Pesanan berhasil dikirim!</p>
  </div>
</template>
