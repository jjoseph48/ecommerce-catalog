<template>
  <div class="container">
    <div v-if="isLoading">
      <!-- Show a loading indicator -->
      <div class="loader"></div>
    </div>
    
    <div v-else class="container" :class="!isProductAvailable ? 'back-gray' : currentProduct.category == 'men\'s clothing' ? 'back-blue' : 'back-pink'">
        <!-- Show product details when not loading -->
        <div class="temp">
          <div v-if="!isProductAvailable" class="container-product-unavailable">
          <div class="product-details">
              <p>This product is unavailable to show</p>
              <div class="footer">
                <button type="button" @click="fetchNextProduct" class="button-next">Next Product</button>
              </div>
            </div>
        </div>

        <div v-else class="container-products">
              <div class="product-image">
                <img :src= "currentProduct.image" alt="">
              </div>

                <div class="product-details">
                <div class="upper">
                    <h1 :class="currentProduct.category === 'men\'s clothing' ? 'blue-font' : 'pink-font'" class="product-title">{{ currentProduct ? currentProduct.title : '' }}</h1>
                  <div class="product-element">
                    <p>Category: {{ currentProduct ? currentProduct.category : '' }}</p>
                    <p>Rating : {{ currentProduct ? currentProduct.rating.rate : '' }}</p>
                  </div>
                  <div>
                    <div class="product-info">
                          <p>{{ currentProduct ? currentProduct.description : '' }}</p>
                        </div>
                    <p>Price : {{ currentProduct ? currentProduct.price : '' }} $</p>
                  </div>
                </div>

              <div class="footer">
                <button type="button" :class="currentProduct.category === 'men\'s clothing' ? 'bg-navy' : 'bg-magenta'"
                          class="button-buy">Buy Now</button>
                <button type="button" @click="fetchNextProduct()"
                          :class="currentProduct.category === 'men\'s clothing' ? 'border-navy font-navy' : 'border-magenta font-magenta'"
                          class="button-next">Next Product</button>
              </div>

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
      isProductAvailable: false,
      isLoading: false,
      currentProduct: {}
    };
  },
  methods: {
    async callProductApi() {
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
  mounted() {
    this.fetchNextProduct();
  }
};
</script>

<style scoped>
@import '../assets/page.css'
</style>
