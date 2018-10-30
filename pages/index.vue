<template>
  <section>
    <div
      :class="['shades', {show: matchedProducts.length && areResultsActive}]"
      @click="areResultsActive = false"
    />
    <search-box
      v-model="searchInput"
      @input="findProducts"
      @focus="areResultsActive = true"
    />
    <transition name="slide">
      <search-results
        v-if="matchedProducts.length && areResultsActive"
        :products="matchedProducts"
      />
    </transition>
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
    matchedProducts: [],
    areResultsActive: false
  }),
  methods: {
    findProducts() {
      const matchedProducts = []
      let searchInput = this.searchInput.toLowerCase().trim()
      if (searchInput.includes('size ')) {
        searchInput = searchInput.split('size ')[1] || ''
        this.filterProducts(products, matchedProducts, product => {
          return this.matchesSize(product, searchInput)
        })
      } else if (searchInput.includes('color ')) {
        searchInput = searchInput.split('color ')[1] || ''
        this.filterProducts(products, matchedProducts, product => {
          return this.matchesColor(product, searchInput)
        })
      } else if (searchInput.includes('style ')) {
        searchInput = searchInput.split('style ')[1] || ''
        this.filterProducts(products, matchedProducts, product => {
          return this.matchesStyle(product, searchInput)
        })
      } else if (searchInput.includes('collection ')) {
        searchInput = searchInput.split('collection ')[1] || ''
        this.filterProducts(products, matchedProducts, product => {
          return this.matchesCollection(product, searchInput)
        })
      } else if (searchInput.length) {
        this.filterProducts(products, matchedProducts, product => {
          return (
            this.matchesTitle(product, searchInput) ||
            this.matchesColor(product, searchInput) ||
            this.matchesSize(product, searchInput) ||
            this.matchesStyle(product, searchInput) ||
            this.matchesCollection(product, searchInput)
          )
        })
      }
      this.matchedProducts = matchedProducts
    },
    filterProducts: (products, matchedProducts, matchCondition) => {
      for (let i = 0; i < products.length; i++) {
        const product = products[i]
        if (matchedProducts.length >= 6) break
        if (matchCondition(product)) {
          matchedProducts.push(product)
        }
      }
    },
    matchesTitle: (product, searchInput) => {
      // If searchInput has a space search
      // query entire title instead of just words
      if (searchInput.includes(' ')) {
        return product.title.toLowerCase().includes(searchInput)
      }
      return product.title
        .toLowerCase()
        .split(' ')
        .some(word => word.startsWith(searchInput))
    },
    matchesColor: (product, searchInput) =>
      product.tags.some(
        tag =>
          tag.startsWith('filter-color')
            ? tag.split('filter-color-')[1] === searchInput
            : false
      ),
    matchesSize: (product, searchInput) =>
      product.options.some(option =>
        option.values.some(val => val === searchInput)
      ),
    matchesStyle: (product, searchInput) =>
      product.tags.some(tag => tag.toLowerCase() === searchInput),
    matchesCollection: (product, searchInput) =>
      product.tags.some(
        tag =>
          tag.startsWith('filter-sortBy')
            ? tag
                .split('filter-sortBy-')[1]
                .split('-')
                .join(' ') === searchInput
            : false
      )
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

.shades {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  height: 100vh;
  width: 100vw;
  background: black;
  opacity: 0;
  visibility: hidden;
  z-index: 0;
  transition: all 0.5s;
}

.show {
  opacity: 0.3;
  visibility: visible;
}

html,
body {
  height: 100%;
  background: ghostwhite;
}

.slide-enter-active {
  animation: slideIn 0.5s forwards;
}
.slide-leave-active {
  animation: slideOut 0.5s forwards;
}

@keyframes slideIn {
  from {
    transform: translateY(10px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes slideOut {
  from {
    transform: translateY(0);
    opacity: 1;
  }
  to {
    transform: translateY(10px);
    opacity: 0;
  }
}
</style>
