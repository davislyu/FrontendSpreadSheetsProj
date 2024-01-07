/MainView.vue
<template>
  <div>
    <WorkbookTabs
      :tabs="tabs"
      :activeTabId="activeTabId"
      @tabChange="setActiveTab"
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

export default {
  components: { WorkbookTabs, Workbook },
  data() {
    return {
      tabs: [{ id: 1, cellContents: {} }],
      activeTabId: 1,
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
    setActiveTab(tabId) {
      this.activeTabId = tabId;
    },

    handleCellUpdate({ row, col, content }) {
      this.activeTabData.cellContents[`${row}-${col}`] = content;
      console.log(content);
    },
  },
};
</script>
