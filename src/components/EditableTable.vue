<template>
  <div @click="handleOutsideClick">
    <el-table
      :data="tableData"
      @cell-click="handleCellClick"
      @cell-mouse-enter="handleCellMouseEnter"
      @cell-mouse-leave="handleCellMouseLeave"
    >
      <template v-for="(column, columnIndex) in columns">
        <el-table-column
          :key="columnIndex"
          :prop="column.prop"
          :label="column.label"
          :width="column.width"
        >
          <template slot-scope="scope">
            <div v-if="isEditing(scope.$index, columnIndex)">
              <el-input
                v-model="scope.row[column.prop]"
                @blur="handleCellBlur(scope.$index, columnIndex)"
              />
            </div>
            <div v-else>{{ scope.row[column.prop] }}</div>
          </template>
        </el-table-column>
      </template>
    </el-table>
  </div>
</template>

<script>
export default {
  props: {
    tableData: {
      type: Array,
      default: () => [],
    },
    columns: {
      type: Array,
      default: () => [],
    },
    editTrigger: {
      type: String,
      default: "click", // 编辑触发方式，默认点击
    },
    editType: {
      type: String,
      default: "cell", // 编辑类型，默认单元格编辑
    },
    beforeEditMethod: {
      type: Function,
      default: () => true, // 编辑前的验证方法，默认允许编辑
    },
  },
  data() {
    return {
      editingRowIndex: -1,
      editingColumnIndex: -1,
    };
  },
  methods: {
    // 判断是否正在编辑指定的单元格
    isEditing(rowIndex, columnIndex) {
      if (this.editType === "cell") {
        return (
          rowIndex === this.editingRowIndex &&
          columnIndex === this.editingColumnIndex
        );
      } else if (this.editType === "row") {
        return rowIndex === this.editingRowIndex;
      }
      return false;
    },
    // 处理单元格点击事件
    handleCellClick(row, column, cell, event) {
      if (this.editTrigger === "click") {
        const rowIndex = this.tableData.indexOf(row);
        const columnIndex = this.columns.findIndex(
          (c) => c.prop === column.property
        );
        if (this.beforeEditMethod({ row, rowIndex, column, columnIndex })) {
          if (this.editType === "cell") {
            this.editingRowIndex = rowIndex;
            this.editingColumnIndex = columnIndex;
          } else if (this.editType === "row") {
            this.editingRowIndex = rowIndex;
          }
        }
      }
    },
    // 处理单元格鼠标进入事件
    handleCellMouseEnter(row, column, cell, event) {
      if (this.editTrigger === "hover") {
        const rowIndex = this.tableData.indexOf(row);
        const columnIndex = this.columns.findIndex(
          (c) => c.prop === column.property
        );
        if (this.beforeEditMethod({ row, rowIndex, column, columnIndex })) {
          if (this.editType === "cell") {
            this.editingRowIndex = rowIndex;
            this.editingColumnIndex = columnIndex;
          } else if (this.editType === "row") {
            this.editingRowIndex = rowIndex;
          }
        }
      }
    },
    // 处理单元格鼠标离开事件
    handleCellMouseLeave(row, column, cell, event) {
      if (this.editTrigger === "hover") {
        this.editingRowIndex = -1;
        this.editingColumnIndex = -1;
      }
    },
    // 处理单元格失去焦点事件
    handleCellBlur(rowIndex, columnIndex) {
      this.editingRowIndex = -1;
      this.editingColumnIndex = -1;
    },
    // 处理点击表格之外的区域
    handleOutsideClick(event) {
      const table = this.$el.querySelector(".el-table");
      if (table && !table.contains(event.target)) {
        this.editingRowIndex = -1;
        this.editingColumnIndex = -1;
      }
    },
  },
};
</script>

<style scoped>
/* 可以在这里添加一些自定义样式 */
</style>
