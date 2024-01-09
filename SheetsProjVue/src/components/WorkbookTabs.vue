<template>
  <div class="tab-navigation-container">
    <ul class="tab-navigation-list">
      <li
        v-for="tab in tabs"
        :key="tab.id"
        :class="{ active: tab.id === activeTabId }"
        @click="setActiveTab(tab.id)"
      >
        Tab {{ tab.id }}
      </li>
      <button @click="addTab" class="add-tab-button">
        <font-awesome-icon icon="fa-solid fa-file-circle-plus" />
      </button>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["tabs", "activeTabId"],
  emits: ["tabChange"],

  methods: {
    setActiveTab(tabId) {
      this.$emit("tabChange", tabId);
    },
    addTab() {
      const newTabId = this.tabs.length + 1;
      this.tabs.push({ id: newTabId, cellContents: {} });
      this.setActiveTab(newTabId);
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/WorkbookTabs.scss";
</style>
