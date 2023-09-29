<template>
  <div>
    <button @click="fetchNextProduct">Next Product</button>
    <div v-if="currentProduct">
      <h2>{{ currentProduct.title }}</h2>
      <p>{{ currentProduct.description }}</p>
      <p>Category: {{ currentProduct.category }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      currentIndex: 1, // Index awal
      currentProduct: null,
    };
  },
  methods: {
    async fetchNextProduct() {
      // Panggil API dengan menggunakan index saat ini
      const response = await fetch(`https://fakestoreapi.com/products/${this.currentIndex}`);
      const data = await response.json();

      if (data && (data.category === "men's clothing" || data.category === "women's clothing")) {
        // Simpan data jika kategori sesuai
        this.currentProduct = data;
      } else {
        this.currentProduct = null;
      }

      // Periksa jika index sudah mencapai 20, atur ulang ke 1
      if (this.currentIndex === 20) {
        this.currentIndex = 1;
      } else {
        this.currentIndex++;
      }
    },
  },
};
</script>
