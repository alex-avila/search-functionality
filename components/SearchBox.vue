<template>
  <div class="search-box__wrapper">
    <input
      :value="value"
      class="search-box__input"
      placeholder="Search"
      @input="$emit('input', $event.target.value)"
      @focus="$emit('focus')"
    >
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ''
    }
  }
}
</script>

<style scoped>
.search-box__wrapper {
  background: rgb(51, 51, 110);
  padding: 2em auto 1em;
  min-height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search-box__input {
  background: rgba(255, 255, 255, 0.75);
  font-size: 20px;
  font-family: 'Avenir';
  border: none;
  outline: none;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
  padding: 0.618em 1em;
  display: block;
  width: 300px;
  padding: 2em auto;
  margin: auto;
  border-radius: 100px;
  transition: all 0.25s ease-out;
}

.search-box__input:focus {
  background: white;
  box-shadow: 0 0 3pt 2pt rgb(64, 89, 233), 0 4px 20px rgba(0, 0, 0, 0.15);
}
</style>
