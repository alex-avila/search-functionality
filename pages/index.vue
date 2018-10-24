<template>
  <section>
    <div
      :class="['shades', {show: matchedProducts.length && areResultsActive}]"
      @click="areResultsActive = false"
    />
    <search-box
      v-model.trim="searchInput"
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
      if (this.searchInput.length >= 3) {
        const searchInput = this.searchInput.toLowerCase()
        this.matchedProducts = products.filter(
          product =>
            product.title.toLowerCase().includes(searchInput) ||
            product.options.some(option =>
              option.values.some(val => val === searchInput)
            ) ||
            product.tags.some(
              tag =>
                tag.startsWith('filter-color')
                  ? tag.includes(searchInput)
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
  opacity: 0.5;
  visibility: visible;
}

html,
body {
  height: 100%;
  background: rgb(39, 39, 39);
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
