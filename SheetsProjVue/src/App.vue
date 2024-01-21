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
* {
  padding: 0;
  margin: 0;
}
.main {
  height: 100vh;
  overflow: hidden;
}
</style>
