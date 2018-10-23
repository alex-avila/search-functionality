<template>
  <section>
    <search-box
      v-model.trim="searchInput"
      @input="findProducts"
    />
    <search-results
      v-if="matchedProducts.length"
      :products="matchedProducts"
    />
  </section>
</template>

<script>
import SearchBox from '@/components/SearchBox.vue'
import SearchResults from '@/components/SearchResults.vue'

import products from '@/static/products.json'

export default {
  components: {
    SearchBox,
    SearchResults
  },
  data: () => ({
    searchInput: '',
    matchedProducts: []
  }),
  methods: {
    findProducts() {
      if (this.searchInput.length >= 3) {
        this.matchedProducts = products.filter(
          product =>
            product.title.toLowerCase().includes(this.searchInput) ||
            product.options.some(option =>
              option.values.some(val => val === this.searchInput)
            ) ||
            product.tags.some(
              tag =>
                tag.startsWith('filter-color')
                  ? tag.includes(this.searchInput)
                  : false
            )
        )
      } else {
        this.matchedProducts = []
      }
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
