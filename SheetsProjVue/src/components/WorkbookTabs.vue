<template>
  <div class="tabs-container">
    <ul class="tabs-list">
      <li
        v-for="tab in tabs"
        :key="tab.id"
        :class="{ active: tab.id === activeTabId }"
        @click="setActiveTab(tab.id)"
      >
        Tab {{ tab.id }}
      </li>
      <button @click="addTab">+</button>
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
<style scoped lang="sass">
.active
  text-decoration: underline
.tabs-list
    list-style: none
    display: flex
    gap: 1rem
    button
      background-color: transparent
      background-repeat: no-repeat
      border: none
      cursor: pointer
      overflow: hidden
      outline: none
      font-size: 1rem
    li:hover,button:hover
        cursor: pointer
</style>
