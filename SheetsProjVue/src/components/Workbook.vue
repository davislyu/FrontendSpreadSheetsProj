//Workbook.vue

<template>
  <div ref="workbook" class="workbook" @scroll="checkScroll">
    <table>
      <tr>
        <th></th>
        <th v-for="col in columns" :key="col">{{ getColumnName(col) }}</th>
      </tr>
      <tr v-for="row in rows" :key="row">
        <th>{{ row }}</th>
        <td v-for="col in columns" :key="col">
          <Cell
            :content="getCellContent(row, col)"
            @updateContent="updateCell(row, col, $event)"
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
    loadMoreRows() {
      let maxRow = this.rows.length;
      for (let i = 1; i <= 10; i++) {
        this.rows.push(maxRow + i);
      }
    },
  },
};
</script>
<style scoped lang="sass">
.workbook
  overflow-x: auto
  height: 100vh

table
  width: 100%
  border-collapse: collapse

th,
td
  border: 1px solid #ddd
  padding: 8px
  text-align: center

.table-header
  background-color: #f0f0f0
</style>
