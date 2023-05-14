<template>
  <div class="container">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="loader-container">
          <div class="loader"></div>
        </div>
      </div>
    </div>
    <div v-else 
      class="container"
      :class="!isProductAvailable ? 'bg-light-gray' : product.data.category === 'men\'s clothing' ? 'bg-light-blue' : 'bg-light-pink'">
      <div class="overlay">
        <img src="../assets/bg-shape.svg" alt="background overlay"/>
      </div>
      <div class="card">
        <div v-if="!isProductAvailable" class="product-unavailable">
          <div class="overlay">
            <img
              alt="unavailable product"
              src="../assets/bg-sad.svg"
              srcset=""/>
          </div>
          <div class="product-details">
            <p>This product is unavailable to show!</p>
            <div class="button-container">
              <button
                class="btn-next"
                type="button"
                @click="getSingleProduct()">
                Next Product
              </button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
          <div class="product-image">
            <img :src="product.data.image" alt=""/>
          </div>
          <div class="product-details">
            <div class="product-title">
              <h3
                class="title"
                :class="product.data.category === 'men\'s clothing' ? 'font-navy' : 'font-pink-dark'">
                {{ product.data.title }}
              </h3>
              <div class="product-sub-title">
                <span>{{ product.data.category }}</span>
                <div class="rating">
                  <span>{{ product.data.rating.rate }}/5</span>
                  <div class="rating">
                    <span
                      class="circle full"
                      :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-pink-dark'">
                    </span>
                    <span
                      class="circle full"
                      :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-pink-dark'">
                    </span>
                    <span
                      class="circle full"
                      :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-pink-dark'">
                    </span>
                    <span
                      class="circle full"
                      :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-pink-dark'">
                    </span>
                    <span
                      class="circle full"
                      :class="product.data.category === 'men\'s clothing' ? 'border-navy' : 'border-pink-dark'" >
                    </span>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>{{ product.data.description }}</p>
              </div>
            </div>
            <div class="price-color">
              <span
                class="price"
                :class="product.data.category === 'men\'s clothing'? 'font-navy': 'font-pink-dark'">
                ${{ product.data.price }}
                </span>
              <div class="button-container">
                <button
                  class="btn-buy"
                  type="button"
                  :class="product.data.category === 'men\'s clothing' ? 'bg-navy' : 'bg-pink-dark'">
                  Buy Now
                </button>
                <button
                  class="btn-next"
                  type="button"
                  @click="getSingleProduct()"
                  :class="product.data.category === 'men\'s clothing' ? 'border-btn-navy font-navy' : 'border-btn-pink-dark font-pink-dark'">
                  Next Product
                </button>
              </div>
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
  data () {
    return {
      product: {},
      isLoading: false,
      isProductAvailable: false,
      index: 0,
    }
  },
  methods: {
    async getSingleProduct () {
      this.isLoading = true

      this.index == 20 ? (this.index = 1) : this.index++

      let data = await this.callAPI()
      if (
        data.category === "women's clothing" ||
        data.category === "men's clothing" 
      ) {
        this.isProductAvailable = true
        this.product = { data }
      } else {
        this.isProductAvailable = false
      }

      this.isLoading = false
    },
    async callAPI () {
      const response = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      )
      const result = await response.json()
      console.log(result)
      return result
    },
  },
  mounted () {
    this.getSingleProduct()
  }
}
</script>
<style>
@import '../assets/style/page.css';
</style>