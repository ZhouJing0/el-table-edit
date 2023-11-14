<template>
  <div>
    <el-table
      :data="list"
      :row-class-name="rowClassName"
      @cell-mouse-enter="handleCellEnter"
      @cell-mouse-leave="handleCellLeave"
    >
      <el-table-column
        prop="date"
        label="日期"
      >
        <template v-slot="scope">
          <el-input
            v-if="scope.row.isEdit"
            v-model="scope.row.date"
            placeholder="请输入内容"
          ></el-input>
          <div v-else class="editable-row__val">{{scope.row.date}}</div>
        </template>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
      >
        <template v-slot="{row}">
          <el-input
            v-if="row.isEdit"
            v-model="row.name"
            placeholder="请输入内容"
          ></el-input>
          <div v-else class="editable-row__val">{{row.name}}</div>
        </template>
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: "RowEdit",
  components: {},
  props: {},
  data() {
    return {
      // 表格数据
      list: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄',
        isEdit: false
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄',
        isEdit: false
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄',
        isEdit: false
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄',
        isEdit: false
      }]
    }
  },
  computed: {
  },
  methods: {
    rowClassName ({ row }) {
      const cls = [
        'editable-row'
      ]
      if (row.isEdit) cls.push('editable-row__current')
      return cls.join(' ')
    },

    /** 鼠标移入cell */
    handleCellEnter (row, column, cell, event) {
      row.isEdit = true
    },
    /** 鼠标移出cell */
    handleCellLeave (row, column, cell, event) {
      row.isEdit = false
    }
  },
  created() {
  },
  mounted() {
  },
  watch: {},
  beforeDestroy() {
  }
}
</script>

<style lang="scss">
.editable-row {
  &.editable-row__current {
    .el-table__cell {
      padding: 4px 0;

      .el-input__inner {
        padding: 0 8px;
        font-size: 12px;
      }
    }
  }

  &__val{
    line-height: 24px;
    padding: 0 9px;
    box-sizing: border-box;
  }
}
</style>
