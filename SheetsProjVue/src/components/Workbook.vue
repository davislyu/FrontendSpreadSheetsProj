<template>
  <div ref="workbook" class="workbook">
    <table>
      <tr class="table-row table-row-headers">
        <th class="table-header table-header-cornerCell"></th>
        <th
          class="table-header table-header-column"
          v-for="col in 20"
          :key="col"
        >
          {{ getColumnName(col) }}
        </th>
      </tr>
      <tr class="table-row" v-for="row in rows" :key="row">
        <th>{{ row }}</th>
        <!-- Row Number -->
        <td class="table-cell" v-for="col in 20" :key="col">
          <Cell
            :initialContent="cellContents[`${row}-${col}`]"
            @updateContent="updateCellContent($event, row, col)"
          />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import Cell from "./Cell.vue";
import { ref, onMounted, onUnmounted } from "vue";

export default {
  components: { Cell },
  setup() {
    const workbook = ref(null);
    const cellContents = ref({});
    const rows = ref(Array.from({ length: 100 }, (_, i) => i + 1)); //Initializing an array with the length of 100

    function getColumnName(col) {
      return String.fromCharCode(64 + col); //Using ASCII to convert nums to letters
    }

    function addMoreRows() {
      if (workbook.value) {
        const element = workbook.value;
        if (
          element.scrollHeight - element.scrollTop <=
          element.clientHeight + 100
        ) {
          const currentRowCount = rows.value.length;
          rows.value.push(
            ...Array.from({ length: 10 }, (_, i) => currentRowCount + i + 1)
          );
        }
      }
    }
    const updateCellContent = (newContent, row, col) => {
      cellContents.value[`${row}-${col}`] = newContent;
    };

    onMounted(() => {
      if (workbook.value) {
        workbook.value.addEventListener("scroll", addMoreRows);
      }
    });

    onUnmounted(() => {
      if (workbook.value) {
        workbook.value.removeEventListener("scroll", addMoreRows);
      }
    });

    return { workbook, rows, cellContents, getColumnName, updateCellContent };
  },
};
</script>

<style scoped>
.workbook {
  overflow-y: auto;
  height: 100vh;
}
table {
  width: 100%;
  border-collapse: collapse;
}
th,
td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: center;
}
.table-header {
  background-color: #f0f0f0;
}
</style>
