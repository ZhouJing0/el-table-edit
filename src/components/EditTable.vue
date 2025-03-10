<template>
  <div>
    <EditableTable
      :tableData="tableData"
      :columns="columns"
      :editTrigger="editTrigger"
      :editType="editType"
      :beforeEditMethod="beforeEditMethod"
    />
  </div>
</template>

<script>
import EditableTable from "./EditableTable.vue";
export default {
  components: {
    EditableTable,
  },
  data() {
    return {
      // 示例数据
      tableData: [
        { name: "张三", age: 20 },
        { name: "李四", age: 22 },
        { name: "王五", age: 25 },
      ],
      // 表格列定义
      columns: [
        { prop: "name", label: "姓名", width: 150 },
        { prop: "age", label: "年龄", width: 100 },
      ],
      // 编辑触发方式
      editTrigger: "hover",
      // 编辑类型
      editType: "cell",
      // 编辑前的验证方法
      beforeEditMethod({ row, rowIndex, column, columnIndex }) {
        console.log("beforeEditMethod", row, rowIndex, column, columnIndex);
        // 示例：只允许编辑年龄列
        return column.property === "age";
      },
    };
  },
};
</script>

<style scoped>
/* 可以在这里添加一些自定义样式 */
</style>
