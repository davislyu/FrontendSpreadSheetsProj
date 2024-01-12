/MainView.vue
<template>
  <div class="main-view-wrapper">
    <Header />
    <WorkbookTabs
      :tabs="tabs"
      :activeTabId="activeTabId"
      @tabChange="setActiveTab"
      @tabToDelete="delTab"
      @addTab="addTab"
    />

    <Workbook
      :cellData="activeTabData.cellContents"
      :rows="rows"
      :columns="columns"
      @cellUpdate="handleCellUpdate"
    />
  </div>
</template>

<script>
import WorkbookTabs from "./WorkbookTabs.vue";
import Workbook from "./Workbook.vue";
import Header from "./Header.vue";

export default {
  components: { WorkbookTabs, Workbook, Header },
  data() {
    return {
      tabs: [
        { id: 1, cellContents: {} },
        { id: 2, cellContents: {} },
        { id: 3, cellContents: {} },
      ],
      activeTabId: 1,
      totalTabsAdded: 3,

      rows: Array.from({ length: 100 }, (_, i) => i + 1),
      columns: 20,
    };
  },
  computed: {
    activeTabData() {
      return this.tabs.find((tab) => tab.id === this.activeTabId) || {};
    },
  },
  methods: {
    addTab() {
      this.totalTabsAdded++;
      const newTabId = this.totalTabsAdded;
      this.tabs.push({ id: newTabId, cellContents: {} });
      this.setActiveTab(newTabId);
    },
    setActiveTab(tabId) {
      this.activeTabId = tabId;
    },
    delTab(tabId) {
      if (this.tabs.length > 1) {
        this.tabs = this.tabs.filter((tab) => tab.id !== tabId);
        if (this.activeTabId === tabId) {
          this.activeTabId = this.tabs.length > 0 ? this.tabs[0].id : null;
        }
      }
    },

    handleCellUpdate({ row, col, content }) {
      this.activeTabData.cellContents[`${row}-${col}`] = content;
      console.log(content);
    },
  },
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
}
.main-view-wrapper {
  width: 100%;
}
</style>
