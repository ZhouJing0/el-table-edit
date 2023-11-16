<template>
  <div id="app">
    <el-radio-group v-model="mode" size="small">
      <el-radio-button label="RowEdit">整行编辑</el-radio-button>
      <el-radio-button label="CellEdit">当前单元格可编辑</el-radio-button>
      <el-radio-button label="ClickEdit">点击进入编辑</el-radio-button>
    </el-radio-group>
    <div style="margin-top: 16px;">
      <component :is="mode"></component>
    </div>
  </div>
</template>

<script>
import RowEdit from './components/RowEdit.vue'
import CellEdit from './components/CellEdit.vue'
import ClickEdit from './components/ClickEdit.vue'

export default {
  name: 'App',
  components: {
    RowEdit,
    CellEdit,
    ClickEdit
  },
  data () {
    return {
      mode: 'RowEdit'
    }
  },
  watch: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
