<template>
  <div class="tabs-container">
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
</template>

<script>
import { ref, watch, defineComponent } from "vue";

export default defineComponent({
  props: {
    tabs: Array,
  },
  setup(props, { emit }) {
    const activeTabId = ref(props.tabs[0].id);

    const addTab = () => {
      const newTabId = props.tabs[props.tabs.length - 1].id + 1;
      emit("addNewTab", newTabId);
      setActiveTab(newTabId);
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
    };
  },
});
</script>

<style lang="sass">
.tabs-list
  display: flex
  gap: 1rem
  li
    list-style: none
    &:hover
      cursor: pointer
  .active
    color: red
</style>
