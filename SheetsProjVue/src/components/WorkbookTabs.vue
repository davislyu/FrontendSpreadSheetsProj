<template>
  <div class="tab-navigation-container">
    <font-awesome-icon
      @click="toggleTabs"
      class="tab-icon burger-icon"
      icon="fa-solid fa-bars"
    />
    <Transition name="slide-fade">
      <ul v-if="tabsActive" class="tab-navigation-list">
        <li
          v-for="tab in tabs"
          :key="tab.id"
          :class="{ active: tab.id === activeTabId }"
          @click="setActiveTab(tab.id)"
        >
          Tab {{ tab.id }}
        </li>
      </ul>
    </Transition>
    <button @click="addTab" class="add-tab-button">
      <font-awesome-icon class="tab-icon plus-icon" icon="fa-solid fa-plus" />
    </button>
  </div>
</template>

<script>
export default {
  props: ["tabs", "activeTabId"],
  emits: ["tabChange"],
  data() {
    return {
      tabsActive: true,
    };
  },
  methods: {
    setActiveTab(tabId) {
      this.$emit("tabChange", tabId);
    },
    addTab() {
      const newTabId = this.tabs.length + 1;
      this.tabs.push({ id: newTabId, cellContents: {} });
      this.setActiveTab(newTabId);
      this.tabsActive = true;
    },
    toggleTabs() {
      this.tabsActive = !this.tabsActive;
    },
  },
};
</script>
<style scoped lang="scss">
@import "../styles/WorkbookTabs.scss";
.slide-fade-enter-active {
  transition: all 0.15s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.15s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(-20px);
  opacity: 0;
}
</style>
