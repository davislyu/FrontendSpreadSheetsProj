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
<style>
.main {
  height: 100vh; /* Ensure the parent container takes full height */
  font-family: "Poppins", sans-serif;
}
</style>
