<template>
  <div>
    <p><strong>Found {{ products.length }} products</strong></p>
    <p
      v-for="product in products"
      :key="product.id"
    >
      {{ product.title }}
    </p>
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
