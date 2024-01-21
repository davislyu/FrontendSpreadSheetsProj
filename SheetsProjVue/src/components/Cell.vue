/Cell.vue
<template>
  <input
    class="cell-input"
    :value="content"
    @input="$emit('updateContent', $event.target.value)"
    @focus="$emit('focus')"
  />
</template>

<script>
export default {
  props: ["content"],
  mounted() {
    this.$emit("cellMounted", this.$el, this.col, this.row);
  },
};
</script>
<style lang="scss">
@import "../styles/Cell.scss";
</style>
