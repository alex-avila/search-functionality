<template>
  <section>
    <search-box
      v-model="searchInput"
      @input="findProducts"
    />
    {{ matchedProducts.length }}
    <p
      v-for="product in matchedProducts"
      :key="product.id"
    >
      {{ product.title }}
    </p>
  </section>
</template>

<script>
import SearchBox from '@/components/SearchBox.vue'

import products from '@/static/products.json'

export default {
  components: {
    SearchBox
  },
  data: () => ({
    searchInput: '',
    matchedProducts: []
  }),
  methods: {
    findProducts() {
      this.matchedProducts = products.filter(product => {
        return product.title.toLowerCase().includes(this.searchInput)
      })
    }
  }
}
</script>

<style>
body {
  max-width: 900px;
  margin: auto;
}
</style>
