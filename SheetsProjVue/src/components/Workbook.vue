//workbook.vue
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
            'cell-container-active': isActiveCell(row, getColumnName(col)),
          }"
        >
          <Cell
            :content="getCellContent(row, getColumnName(col))"
            @updateContent="updateCell(row, getColumnName(col), $event)"
            @focus="setFocusedCell(row, getColumnName(col))"
            @cellMounted="cellMountedHandler($event, getColumnName(col), row)"
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
      activeCell: null,
    };
  },
  props: ["cellData", "rows", "columns"],
  mounted() {
    this.loadMoreRows();
    eventBus.on("tabChanged", () => {
      this.focusedCell = null;
    });
    document.addEventListener("keydown", this.handleArrowKeys);
  },
  beforeUnmount() {
    document.removeEventListener("keydown", this.handleArrowKeys);
  },
  methods: {
    saveDataToLocalStorage() {
      localStorage.setItem("workbookData", JSON.stringify(this.cellData));
    },
    clearActiveCell() {
      console.log("clearActiveCell called");
      this.focusedCell = null;
      this.activeCell = null;
    },

    getColumnName(col) {
      let columnName = "";
      let dividend = col;
      let modulo;

      while (dividend > 0) {
        modulo = (dividend - 1) % 26;
        columnName = String.fromCharCode(65 + modulo) + columnName;
        dividend = Math.floor((dividend - modulo) / 26);
      }

      return columnName;
    },
    handleArrowKeys(event) {
      if (!this.focusedCell) return;

      let [col, row] = this.focusedCell.split(":");
      row = parseInt(row);

      switch (event.key) {
        case "ArrowLeft":
          col = this.getPreviousColumn(col);
          break;
        case "ArrowRight":
          col = this.getNextColumn(col);
          break;
        case "ArrowUp":
          row = Math.max(1, row - 1);
          break;
        case "ArrowDown":
          row += 1;
          break;
      }
      this.setFocusedCell(row, col);
      this.setActiveCell(row, col);
    },
    getPreviousColumn(col) {
      const colIndex = col.charCodeAt(0) - 65;
      if (colIndex > 0) {
        return String.fromCharCode(65 + colIndex - 1);
      }
      return col;
    },
    cellMountedHandler(cellElement, col, row) {
      if (this.isActiveCell(row, col)) {
        const inputElement = cellElement.querySelector("input");
        if (inputElement) {
          inputElement.focus();
        }
      }
    },
    getNextColumn(col) {
      const colIndex = col.charCodeAt(0) - 65;
      if (colIndex < this.columns - 1) {
        return String.fromCharCode(65 + colIndex + 1);
      }
      return col;
    },
    getCellContent(row, col) {
      return (this.cellData[row] && this.cellData[row][col]) || "";
    },

    updateCell(row, col, content) {
      if (!this.cellData[row]) {
        this.cellData[row] = {};
      }
      this.cellData[row][col] = content;
      this.saveDataToLocalStorage();
    },
    isActiveCell(row, col) {
      return this.activeCell === `${col}:${row}`;
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
      row = Math.max(1, Math.min(this.rows.length, row));
      this.focusedCell = `${col}:${row}`;
      eventBus.emit("focusedCellChange", this.focusedCell);
    },
    setActiveCell(row, col) {
      this.activeCell = `${col}:${row}`;
      this.$nextTick(() => {
        const activeCellElement = this.$refs.workbook.querySelector(
          `.cell-container-active`
        );
        if (activeCellElement) {
          activeCellElement.querySelector("input").focus();
        }
      });
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
