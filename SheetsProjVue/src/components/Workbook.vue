<template>
  <div ref="workbook" class="workbook">
    <table>
      <tr class="table-headers">
        <th class="table-header"></th>
        <!-- Empty cell for the corner -->
        <th class="table-header" v-for="col in 20" :key="col">
          {{ getColumnName(col) }}
        </th>
      </tr>
      <tr v-for="row in rows" :key="row">
        <th>{{ row }}</th>
        <!-- Row Header -->
        <td v-for="col in 20" :key="col">
          <input type="text" v-model="cellContents[`${row}-${col}`]" />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted } from "vue";

export default {
  setup() {
    const workbook = ref(null);
    const rows = ref(Array.from({ length: 100 }, (_, i) => i + 1));
    const cellContents = ref({});

    function getColumnName(col) {
      return String.fromCharCode(64 + col); // ASCII 65 is 'A'
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

    return { workbook, rows, cellContents, getColumnName };
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
