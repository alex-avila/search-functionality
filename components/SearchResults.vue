<template>
  <div class="search-results__wrapper">
    <div class="search-results">
      <!-- <transition-group
        name="products"
        class="search-results"
      > -->
      <div
        v-for="product in products"
        :key="product.id"
      >
        <search-result
          :title="product.title"
          :src="product.images[0].src"
          :alt="product.images[0].altText || product.title"
        />
      </div>
      <!-- </transition-group> -->
    </div>
  </div>
</template>

<script>
import SearchResult from './SearchResult.vue'

export default {
  components: {
    SearchResult
  },
  props: {
    products: {
      type: Array,
      required: true,
      default: () => []
    }
  }
}
</script>

<style>
.search-results__wrapper {
  max-width: 700px;
  margin: auto;
}

.search-results {
  background: ghostwhite;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.158);
  padding: 1em;
  border-radius: 4px;
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1em;
}

/* .products-enter-active,
.products-leave-active {
  transition: all 0.25s;
}
.products-enter,
.products-leave-to {
  opacity: 0;
}
.products-move {
  transition: opacity 0.25s;
}
.products-leave-active {
  position: absolute;
} */
</style>
