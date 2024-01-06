//App.vue
<template>
  <div class="main">
    <MainView />
  </div>
</template>
<script>
import MainView from "./components/MainView.vue";
export default {
  components: { MainView },
};
</script>
<style scoped>
.main {
  height: 100vh; /* Ensure the parent container takes full height */
}
</style>
