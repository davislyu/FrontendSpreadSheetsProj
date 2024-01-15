<template>
  <div ref="workbook" class="workbook" @scroll="checkScroll">
    <table class="workbook-table">
      <tr class="table-header-row">
        <th id="corner-cell" class="table-header-cell"></th>
        <th
          class="table-header-cell"
          v-for="col in columns"
          :key="col"
          :class="{
            'active-header-cell': isHeaderFocused(getColumnName(col), null),
          }"
        >
          {{ getColumnName(col) }}
        </th>
      </tr>
      <tr v-for="row in rows" :key="row" class="table-row">
        <th
          class="row-header-cell"
          :class="{ 'active-header-cell': isHeaderFocused(null, row) }"
        >
          <p class="row-header-label">{{ row }}</p>
        </th>
        <td
          v-for="col in columns"
          :key="col"
          class="cell-container"
          :class="{
            'cell-container-focused': isCellFocused(row, getColumnName(col)),
          }"
        >
          <Cell
            :content="getCellContent(row, getColumnName(col))"
            @updateContent="updateCell(row, getColumnName(col), $event)"
            @focus="setFocusedCell(row, getColumnName(col))"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import Cell from "./Cell.vue";
import { eventBus } from "../eventBus";
export default {
  components: { Cell },
  data() {
    return {
      focusedCell: null,
    };
  },

  props: ["cellData", "rows", "columns"],
  mounted() {
    this.loadMoreRows();
    eventBus.on("tabChanged", () => {
      this.focusedCell = null;
    });
  },
  methods: {
    saveDataToLocalStorage() {
      localStorage.setItem("workbookData", JSON.stringify(this.cellData));
    },
    getColumnName(col) {
      return String.fromCharCode(64 + col);
    },

    getCellContent(row, col) {
      const key = `${col}${row}`;
      return this.cellData[key] || "";
    },

    updateCell(row, col, content) {
      this.$emit("cellUpdate", { col, row, content });
      this.saveDataToLocalStorage();
    },
    checkScroll() {
      const { scrollTop, scrollHeight, clientHeight } = this.$refs.workbook;
      if (scrollTop + clientHeight >= scrollHeight - 100) {
        this.loadMoreRows();
      }
    },

    isCellFocused(row, col) {
      return this.focusedCell === `${col}:${row}`;
    },
    loadMoreRows() {
      let maxRow = this.rows.length;
      for (let i = 1; i <= 10; i++) {
        this.rows.push(maxRow + i);
      }
    },
    setFocusedCell(row, col) {
      this.focusedCell = `${col}:${row}`;
      eventBus.emit("focusedCellChange", this.focusedCell);
    },

    isHeaderFocused(col, row) {
      if (!this.focusedCell) return false;

      const [focusedCol, focusedRow] = this.focusedCell.split(":");
      return (
        (col && focusedCol === col) || (row && focusedRow === row.toString())
      );
    },
  },
};
</script>

<style lang="scss">
@import "../styles/Workbook.scss";
</style>
