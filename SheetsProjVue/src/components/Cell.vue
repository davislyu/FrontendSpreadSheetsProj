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
};
</script>
<style lang="scss">
@import "../styles/Cell.scss";
</style>
