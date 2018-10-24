<template>
  <div class="search-results__wrapper">
    <div class="search-results">
      <p><strong>Found {{ products.length }} products</strong></p>
      <p
        v-for="product in products"
        :key="product.id"
      >
        {{ product.title }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
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
  max-width: 800px;
  margin: auto;
}

.search-results {
  background: ghostwhite;
  box-shadow: 0 2px 0 rgba(255, 255, 255, 0.5), 0 2px 4px rgba(0, 0, 0.25);
  padding: 1em;
  border-radius: 10px;
  position: relative;
  z-index: 1;
}
</style>
