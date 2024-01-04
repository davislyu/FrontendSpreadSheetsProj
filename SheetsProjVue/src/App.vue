<template>
  <div class="main">
    <mainView />
  </div>
</template>
<script>
import mainView from "./components/Workbook.vue";
export default {
  components: { mainView },
};
</script>
<style scoped>
.main {
  height: 100vh; /* Ensure the parent container takes full height */
}
</style>
