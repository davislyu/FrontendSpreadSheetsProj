/Cell.vue
<template>
  <textarea
    class="cell-input"
    :value="content"
    @input="$emit('updateContent', $event.target.value)"
  />
</template>

<script>
export default {
  props: ["content"],
};
</script>
<style lang="sass">
.cell-input
  width: 8vw
  resize: none
  border: none
  &:focus
    outline: none
</style>
