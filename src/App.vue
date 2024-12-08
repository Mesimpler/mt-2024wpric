<template>
  <h4 class="result-alert">
    总共获取 {{ totalPoint }} 积分，预计收入 {{ income }} 元
  </h4>

  <el-table
    :data="tableData"
    style="width: 100%"
    :border="true"
    :header-cell-style="{ 'text-align': 'center' }"
    :cell-style="{ 'text-align': 'center' }"
  >
    <el-table-column prop="part" label="期数"> </el-table-column>
    <el-table-column prop="grad" label="计划挡位">
      <template #default="{ row }">
        <el-select v-model="row.grad" placeholder="请选择">
          <el-option
            v-for="item in grads"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </template>
    </el-table-column>
    <el-table-column prop="boost" label="该期翻倍率">
      <template #default="{ row }"> x{{ row.boost }} </template>
    </el-table-column>
    <el-table-column prop="point" label="该期积分">
      <template #default="{ row }">
        {{ row.grad * row.boost }}
      </template>
    </el-table-column>
  </el-table>
</template>

<script setup>
import { ref, computed, watch, onBeforeMount } from "vue";

const grads = [
  {
    value: 0,
    label: "0档",
  },
  {
    value: 1,
    label: "1档",
  },
  {
    value: 2,
    label: "2档",
  },
  {
    value: 3,
    label: "3档",
  },
];

const tableData = ref([
  {
    part: "第一期",
    grad: 1,
    boost: 1,
  },
  {
    part: "第二期",
    grad: 1,
    boost: 1,
  },
  {
    part: "第三期",
    grad: 1,
    boost: 2,
  },
  {
    part: "第四期",
    grad: 1,
    boost: 2,
  },
  {
    part: "第五期",
    grad: 1,
    boost: 2,
  },
  {
    part: "第六期",
    grad: 1,
    boost: 2,
  },
  {
    part: "第七期",
    grad: 1,
    boost: 3,
  },
  {
    part: "第八期",
    grad: 1,
    boost: 3,
  },
  {
    part: "第九期",
    grad: 1,
    boost: 3,
  },
  {
    part: "第十期",
    grad: 1,
    boost: 3,
  },
]);

onBeforeMount(() => {
  const localTableData = localStorage.getItem("tableData");
  if (localTableData !== null) {
    tableData.value = JSON.parse(localTableData);
  }
});

watch(
  tableData,
  (newTableData) => {
    localStorage.setItem("tableData", JSON.stringify(newTableData));
  },
  { deep: true }
);

const totalPoint = computed(() => {
  return tableData.value.reduce((accumulator, item) => {
    return accumulator + item.grad * item.boost;
  }, 0);
});

const income = computed(() => {
  if (totalPoint.value < 16) {
    return 0;
  } else if (16 <= totalPoint.value && totalPoint.value <= 30) {
    return totalPoint.value * 24;
  } else if (31 <= totalPoint.value && totalPoint.value <= 47) {
    return totalPoint.value * 36;
  } else if (48 <= totalPoint.value && totalPoint.value <= 65) {
    return totalPoint.value * 72;
  } else if (66) {
    return 4788;
  }
});
</script>

<style scoped>
.result-alert {
  text-align: center;
}
</style>
