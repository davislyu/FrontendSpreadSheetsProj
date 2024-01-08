/Cell.vue
<template>
  <textarea
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
<style lang="sass">
.cell-input
  font-family: "Poppins", sans-serif
  width: 8vw
  font-size: 1rem
  resize: none
  border: none
  &:focus
    outline: none
</style>
