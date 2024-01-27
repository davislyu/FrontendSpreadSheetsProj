//Header.vue
<template>
  <div class="header-container">
    <header class="header-title-section">
      <i class="header-title-sheetsLogo">
        <img src="../assets/sheetsLogo.png" alt="#" />
      </i>
      <input
        type="text"
        placeholder="United Tabular Sheet"
        class="header-title-text"
        @input="adjustWidth"
        :size="inputSize"
        @click="clearWorkbookFocus"
        ref="input"
      />
    </header>
    <div class="header-pfPic">
      <img src="../assets/boris.png" alt="#" />
    </div>
  </div>
</template>

<script>
import { eventBus } from "../eventBus";
export default {
  data() {
    return {
      inputSize: 1,
    };
  },

  methods: {
    /**
     * Adjusts the width of the title input field based on its content length.
     */
    adjustWidth() {
      let input = this.$refs.input;
      this.inputSize =
        Math.max(input.value.length, input.placeholder.length) || 10;
    },

    clearWorkbookFocus() {
      eventBus.emit("clearCellFocus");
    },
  },
  mounted() {
    this.adjustWidth();
  },
};
</script>

<style lang="scss" scoped>
@import "../styles/Header.scss";
</style>
