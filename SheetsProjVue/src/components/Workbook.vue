//Workbook.vue

<template>
  <div ref="workbook" class="workbook-container" @scroll="checkScroll">
    <table class="workbook-table">
      <tr class="table-header-row">
        <th class="table-header-cell"></th>
        <th
          class="table-header-cell"
          v-for="col in columns"
          :key="col"
          :class="{ 'active-header-cell': isHeaderFocused(null, col) }"
        >
          {{ getColumnName(col) }}
        </th>
      </tr>
      <tr v-for="row in rows" :key="row" class="table-row">
        <th
          class="row-header-cell"
          :class="{ 'active-header-cell': isHeaderFocused(row, null) }"
        >
          <p class="row-header-label">{{ row }}</p>
        </th>
        <td
          v-for="col in columns"
          :key="col"
          class="cell-container"
          :class="{ 'cell-container-focused': isCellFocused(row, col) }"
        >
          <Cell
            :content="getCellContent(row, col)"
            @updateContent="updateCell(row, col, $event)"
            @focus="setFocusedCell(row, col)"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import Cell from "./Cell.vue";

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
  },
  methods: {
    getColumnName(col) {
      return String.fromCharCode(64 + col);
    },
    getCellContent(row, col) {
      return this.cellData[`${row}-${col}`] || "";
    },
    updateCell(row, col, content) {
      this.$emit("cellUpdate", { row, col, content });
    },
    checkScroll() {
      const { scrollTop, scrollHeight, clientHeight } = this.$refs.workbook;
      if (scrollTop + clientHeight >= scrollHeight - 100) {
        this.loadMoreRows();
      }
    },
    isCellFocused(row, col) {
      return this.focusedCell === `${row}-${col}`;
    },
    loadMoreRows() {
      let maxRow = this.rows.length;
      for (let i = 1; i <= 10; i++) {
        this.rows.push(maxRow + i);
      }
    },
    setFocusedCell(row, col) {
      this.focusedCell = `${row}-${col}`;
    },
    isHeaderFocused(row, col) {
      if (!this.focusedCell) return false;
      const [focusedRow, focusedCol] = this.focusedCell.split("-");
      return (
        (row && focusedRow === row.toString()) ||
        (col && focusedCol === col.toString())
      );
    },
  },
};
</script>
<style lang="scss">
@import "../styles/Workbook.scss";
</style>
