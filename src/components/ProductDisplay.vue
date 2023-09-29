<template>
  <div class="container">
    <div class="container-products">
      <div>

      </div>
      <div>
        <h1>Name of Product</h1>
        <div>
          <p>Category</p>
          <p>Rates</p>
        </div>
        <div>
          <p>Products's Details</p>
          <p>Price</p>
        </div>

        <div>
          <button>Buy</button>
          <button @click="fetchNextProduct">Next Product</button>
          <div v-if="isLoading">
            <!-- Show a loading indicator -->
            <div class="loader"></div>
          </div>
          <div v-else>
            <!-- Show product details when not loading -->
            <h2>{{ currentProduct ? currentProduct.title : '' }}</h2>
            <p>{{ currentProduct ? currentProduct.description : '' }}</p>
            <p>Category: {{ currentProduct ? currentProduct.category : '' }}</p>
          </div>
        </div>

      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: 'ProductDisplay',
  data() {
    return {
      currentIndex: 0, // Index awal
      isProductAvailable : false,
      isLoading: false,
      product: {}
    };
  },
  methods: {
    async callProductApi(){
      // Panggil API dengan menggunakan index saat ini
      const response = await fetch(`https://fakestoreapi.com/products/${this.currentIndex}`);
      const data = await response.json();
      return data;
    },

    async fetchNextProduct() {
      this.isLoading = true;

      // Periksa jika index sudah mencapai 20, atur ulang ke 1
      if (this.currentIndex === 20) {
        this.currentIndex = 1;
      } else {
        this.currentIndex++;
      }

      let data = await this.callProductApi()
      if (data && (data.category === "men's clothing" || data.category === "women's clothing")) {
        // Simpan data jika kategori sesuai
        this.currentProduct = data;
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }
      this.isLoading = false;
    }
  },
  mounted(){
    this.fetchNextProduct();
  }
};
</script>

<style scoped>
  @import '../assets/page.css'
</style>
