//Workbooktabs.vue
<template>
  <div class="tab-wrapper">
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
            <span @click.stop="tabToDelete(tab.id)">
              <font-awesome-icon
                class="tab-close-btn"
                icon="fa-solid fa-xmark"
              />
            </span>
          </li>
        </ul>
      </Transition>
      <button @click="addTab" class="add-tab-button">
        <font-awesome-icon class="tab-icon plus-icon" icon="fa-solid fa-plus" />
      </button>
    </div>
    <div class="tab-settings">
      <form class="FormContainer" action="submit">
        <input
          id="cell-finder-input"
          type="text"
          :placeholder="current_active_cell"
        />
        <button @click.prevent="addNewColumn" class="add-column-button">
          Add Column
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { eventBus } from "../eventBus";

export default {
  props: ["tabs", "activeTabId"],
  emits: ["tabChange", "tabToDelete"],
  data() {
    return {
      tabsActive: true,
      current_active_cell: "",
    };
  },

  methods: {
    setActiveTab(tabId) {
      this.$emit("tabChange", tabId);
      eventBus.emit("tabChanged");
      this.current_active_cell = null;
    },

    addTab() {
      this.$emit("addTab");
      this.tabsActive = true;
    },

    addNewColumn() {
      this.$emit("addColumn");
    },

    toggleTabs() {
      this.tabsActive = !this.tabsActive;
    },

    tabToDelete(tabId) {
      this.$emit("tabToDelete", tabId);
    },
  },
  mounted() {
    eventBus.on("focusedCellChange", (data) => {
      this.current_active_cell = data;
    });
  },

  unmounted() {
    eventBus.off("focusedCellChange");
  },
};
</script>
<style scoped lang="scss">
@import "../styles/WorkbookTabs.scss";
</style>
