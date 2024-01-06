//Cell.vue
<template>
  <input class="cell-input" type="text" v-model="content" />
</template>

<script>
export default {
  name: "Cell",
  props: {
    initialContent: String,
  },
  data() {
    return {
      content: this.initialContent,
    };
  },
  watch: {
    initialContent(newValue) {
      this.content = newValue;
    },
    content() {
      this.updateContent();
    },
  },
  methods: {
    updateContent() {
      this.$emit("updateContent", this.content);
    },
  },
};
</script>

<style scoped>
.cell-input {
  width: 6vw;
}
</style>
