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
      @addColumn="handleAddColumn"
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
import { eventBus } from "../eventBus";

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
  mounted() {
    this.loadDataFromLocalStorage();
    eventBus.on("tabChanged", this.loadDataFromLocalStorage);
  },
  beforeUnmount() {
    eventBus.off("tabChanged", this.loadDataFromLocalStorage);
  },
  methods: {
    addTab() {
      this.totalTabsAdded++;
      const newTabId = this.totalTabsAdded;
      this.tabs.push({ id: newTabId, cellContents: {} });
      this.setActiveTab(newTabId);
    },
    loadDataFromLocalStorage() {
      const savedData = localStorage.getItem("workbookTabsData");
      if (savedData) {
        const tabsData = JSON.parse(savedData);
        this.tabs.forEach((tab) => {
          if (tabsData[tab.id]) {
            tab.cellContents = tabsData[tab.id];
          }
        });
      }
    },
    handleAddColumn() {
      this.columns += 1;
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

    handleCellUpdate({ col, row, content }) {
      this.activeTabData.cellContents[`${col}${row}`] = content;
      const tabsData = this.tabs.reduce((acc, tab) => {
        acc[tab.id] = tab.cellContents;
        return acc;
      }, {});
      localStorage.setItem("workbookTabsData", JSON.stringify(tabsData));
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
  height: 100%;
  display: flex;
  flex-direction: column;
}
</style>
