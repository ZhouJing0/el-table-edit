<template>
  <div>
    <el-table
      :data="tableData"
      @cell-mouse-enter="handleCellEnter"
      @cell-mouse-leave="handleCellLeave"
      @cell-click="handleCellClick"
      :cell-style="{padding: '4px 0'}"
    >
      <el-table-column
        prop="date"
        label="日期"
      >
        <template v-slot="{row}">
          <el-input class="editable-cell__input" v-model="row.date" placeholder="请输入内容"></el-input>
          <div class="editable-cell__val">{{row.date}}</div>
        </template>
      </el-table-column>
      <el-table-column
        prop="name"
        label="姓名"
      >
        <template v-slot="{row}">
          <el-input class="editable-cell__input" v-model="row.name" placeholder="请输入内容"></el-input>
          <div class="editable-cell__val">{{row.name}}</div>
        </template>
      </el-table-column>
      <el-table-column
        prop="food"
        label="食物"
      >
        <template v-slot="{row}">
          <el-select class="editable-cell__input" v-model="row.food" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          <div class="editable-cell__val">{{row.food | foodLabel }}</div>
        </template>
      </el-table-column>
      <el-table-column
        label="操作"
      >
        <template v-slot="{row}">
          <el-button @click="save(row)" :disabled="!row.isEdit" type="text" size="small">保存</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
function mapToOptions (map) {
  if (map instanceof Map) {
    return [...map.entries()].map(([key, value]) => ({
      label: value,
      value: key
    }))
  }
  return []
}

const optionsMap = new Map([
  ['选项1', '黄金糕'],
  ['选项2', '双皮奶'],
  ['选项3', '蚵仔煎'],
  ['选项4', '龙须面'],
  ['选项5', '北京烤鸭'],
])
export default {
  name: "ClickEdit",
  components: {},
  props: {},
  filters: {
    foodLabel (val) {
      return optionsMap.get(val)
    }
  },
  data () {
    return {
      // 下拉选项
      options: mapToOptions(optionsMap),
      // 表格数据
      tableData: [{
        id: 0,
        date: '2016-05-02',
        name: '王小虎',
        food: '选项5',
        isEdit: false
      }, {
        id: 1,
        date: '2016-05-04',
        name: '王小虎',
        food: '选项5',
        isEdit: false
      }, {
        id: 2,
        date: '2016-05-01',
        name: '王小虎',
        food: '选项5',
        isEdit: false
      }, {
        id: 3,
        date: '2016-05-03',
        name: '王小虎',
        food: '选项5',
        isEdit: false
      }],
      // 需要编辑的属性
      editProp: ['date', 'name', 'food'],
      // 保存进入编辑的cell
      editCells: new Map()
    }
  },
  computed: {
  },
  methods: {
    /** 鼠标移入cell */
    handleCellEnter (row, column, cell, event) {
      const property = column.property
      if (this.editProp.includes(property)) {
        cell.querySelector('.editable-cell__val').classList.add('editable-cell__val--hover')
      }
    },
    /** 鼠标移出cell */
    handleCellLeave (row, column, cell, event) {
      const property = column.property
      if (this.editProp.includes(property)) {
        cell.querySelector('.editable-cell__val').classList.remove('editable-cell__val--hover')
      }
    },
    /** 点击cell */
    handleCellClick (row, column, cell, event) {
      const property = column.property
      if (this.editProp.includes(property)) {
        // 保存cell
        this.saveCellClick(row, cell)
        cell.querySelector('.editable-cell__val').style.display = 'none'
        cell.querySelector('.editable-cell__input').style.display = 'block'
        cell.querySelector('input').focus()
      }
    },
    /** 取消编辑状态 */
    cancelEditable (cell) {
      cell.querySelector('.editable-cell__val').style.display = 'block'
      cell.querySelector('.editable-cell__input').style.display = 'none'
    },
    /** 保存进入编辑的cell */
    saveCellClick (row, cell) {
      if (this.editCells.has(row)) {
        this.editCells.get(row).add(cell)
      } else {
        this.editCells.set(row, new Set([cell]))
      }
      this.updateRowSave(row)
    },
    /** 保存数据 */
    save (row) {
      this.editCells.get(row).forEach(cell => {
        this.cancelEditable(cell)
      })
      this.editCells.delete(row)
      this.updateRowSave(row)
    },
    updateRowSave (row) {
      row.isEdit = this.editCells.has(row)
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

<style lang="scss" scoped>
.editable-cell__input {
  display: none;
  .el-input__inner {
    padding: 0 8px;
    font-size: 12px;
  }
}

.editable-cell__val {
  box-sizing: border-box;
  height: 32px;
  line-height: 32px;
  border: 1px solid transparent;

  &--hover{
    border-color: #dddddd;
    border-radius: 4px;
    cursor: text;
  }
}
</style>
