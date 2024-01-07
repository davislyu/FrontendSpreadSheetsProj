//workbookTabs.vue
<template>
  <div class="main-container">
    <div>
      <button @click="toggleActiveTabStatus">
        {{ tabsActive ? "Hide tabs" : "Open tabs" }}
      </button>
    </div>
    <div v-if="tabsActive" class="tabs-container">
      <ul class="tabs-list">
        <li
          v-for="tab in tabs"
          :key="tab.id"
          :class="{ active: tab.id === activeTabId }"
          @click="() => setActiveTab(tab.id)"
        >
          Tab {{ tab.id }}
        </li>
        <button @click="addTab">+</button>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, watch, defineComponent } from "vue";

export default defineComponent({
  props: {
    tabs: Array,
  },
  setup(props, { emit }) {
    const activeTabId = ref(props.tabs[0].id);
    const tabsActive = ref(true);

    const addTab = () => {
      const newTabId = props.tabs[props.tabs.length - 1].id + 1;
      emit("addNewTab", newTabId);
      setActiveTab(newTabId);
    };

    const toggleActiveTabStatus = () => {
      tabsActive.value = !tabsActive.value;
    };

    const setActiveTab = (tabId) => {
      activeTabId.value = tabId;
      emit("tabChanged", tabId);
    };

    watch(
      () => props.tabs,
      (newTabs) => {
        if (!newTabs.find((tab) => tab.id === activeTabId.value)) {
          activeTabId.value = newTabs[0].id;
        }
      }
    );

    return {
      activeTabId,
      setActiveTab,
      addTab,
      tabsActive,
      toggleActiveTabStatus,
    };
  },
});
</script>

<style scoped lang="sass">
.tabs-list
  display: flex
  gap: 1rem
  li
    list-style: none
    &:hover
      cursor: pointer
  button
    border: none
    background-color: transparent
    font-size: 1rem
    &:hover
        cursor: pointer
  .active
    text-decoration: underline
.main-container
    display: flex
    align-items: center
    padding: 0
    margin: 0
</style>
