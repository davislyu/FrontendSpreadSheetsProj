<template>
  <input id="cell" type="text" v-model="content" />
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
  methods: {
    updateContent() {
      this.$emit("updateContent", this.content);
    },
  },
  watch: {
    content() {
      this.updateContent();
    },
  },
};
</script>

<style scoped>
#cell {
  width: 6vw;
}
</style>
