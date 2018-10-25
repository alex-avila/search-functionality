<template>
  <div class="search-result">
    <div
      :src="src"
      :alt="alt"
      :style="imgStyle"
    />
    <p class="product-title">{{ title }}</p>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    src: {
      type: String,
      required: true
    },
    alt: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      imgStyle: {
        background: `url(${this.src})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        backgroundRepeat: 'no-repeat',
        borderRadius: '4px',
        height: '100px',
        width: '100px'
      }
    }
  }
}
</script>

<style scoped>
.search-result {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 1em;
  align-items: center;
}
.product-title {
  font-weight: bold;
}
</style>
