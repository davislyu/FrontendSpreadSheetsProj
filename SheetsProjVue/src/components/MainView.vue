<template>
  <div>
    <WorkbookTabs
      :tabs="tabs"
      @tabChanged="handleTabChange"
      @addNewTab="addTab"
    />
    <Workbook
      :rows="activeTabData.rows"
      :cellContents="activeTabData.cellContents"
    />
  </div>
</template>

<script>
import WorkbookTabs from "./WorkbookTabs.vue";
import Workbook from "./Workbook.vue";
import { ref, reactive } from "vue";

export default {
  components: {
    WorkbookTabs,
    Workbook,
  },
  setup() {
    const tabs = reactive([
      {
        id: 1,
        rows: Array.from({ length: 100 }, (_, i) => i + 1),
        cellContents: {},
      },
      {
        id: 2,
        rows: Array.from({ length: 100 }, (_, i) => i + 1),
        cellContents: {},
      },
    ]);
    const activeTabData = ref(tabs[0]);

    function addTab(newTabId) {
      const newTab = {
        id: newTabId,
        rows: Array.from({ length: 100 }, (_, i) => i + 1),
        cellContents: {},
      };
      tabs.push(newTab);
      activeTabData.value = newTab;
    }

    function handleTabChange(tabId) {
      const newActiveTab = tabs.find((tab) => tab.id === tabId);
      if (newActiveTab) {
        activeTabData.value = newActiveTab;
      }
    }

    return {
      tabs,
      activeTabData,
      handleTabChange,
      addTab,
    };
  },
};
</script>
