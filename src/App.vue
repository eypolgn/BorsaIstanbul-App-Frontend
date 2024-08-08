<template>
  <div id="app">
    <div class="container">
      <StockDetails />
    </div>
  </div>
</template>

<script>
import StockDetails from "./components/StockDetails.vue";

export default {
  components: {
    StockDetails,
  },
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  padding: 20px;
}

.container {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 600px; 
}

h3 {
  margin-bottom: 10px;
}

pre {
  white-space: pre-wrap;
  word-wrap: break-word;
}
</style>
