<template>
  <div id="app">
    <el-form :inline="true" :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="行数" prop="row" required>
        <el-input v-model="ruleForm.row" placeholder="请输入行数"></el-input>
      </el-form-item>
    <el-form-item label="列数" prop="col" required>
      <el-input v-model="ruleForm.col" placeholder="请输入列数"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="createTable('ruleForm')">创建表格</el-button>
    </el-form-item>
    </el-form>

    <el-table
    v-show="showTable"
    ref="table"
    :data="tableData"
    highlight-current-row
    style="width: 100%"
    @cell-click="handleClick">
    <el-table-column
      v-for="item in colList"
      :label="tableData[0][item]"
      width="100"
      :key="item"
      align="center"
      :prop="item"
      >
      <!-- :property="item" -->
      <template slot-scope="scope">
        <el-button type="text" size="small" @click="print(scope)">{{scope.row[item]}}</el-button>
      </template>
    </el-table-column>
  </el-table>
  </div>
</template>

<script type="text/javascript">
export default {
  name: 'App',
  data () {
    return {
      ruleForm: {
        row: '',
        col: ''
      },
      rules: {
        row: [
          { required: true, message: '请输入行数', trigger: 'blur' },
          { min: 1, message: '行数至少大于1', trigger: 'blur' }
        ],
        col: [
          { required: true, message: '请输入列数', trigger: 'blur' },
          { min: 1, message: '列数至少大于1', trigger: 'blur' }
        ]
      },
      showTable: false,
      tableData: [],
      colList: []
    }
  },
  methods: {
    print (scope) {
      console.log(scope)
    },
    createTable (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.showTable = true
          var arr = []
          for (let i = 0; i < this.ruleForm.row; i++) {
            let oneRow = {}
            for (let j = 0; j < this.ruleForm.col; j++) {
              oneRow[String(j)] = String(j)
            }
            arr.push(oneRow)
          }
        }
        this.tableData = arr
        var colList = []
        for (let j = 0; j < this.ruleForm.col; j++) {
          colList.push(String(j))
        }
        this.colList = colList
      })
    },
    hasClass (ele, cls) {
      return ele.className.match(new RegExp('(\\s|^)' + cls + '(\\s|$)'))
    },
    // 为指定的dom元素添加样式
    addClass (ele, cls) {
      if (!this.hasClass(ele, cls)) ele.className += ' ' + cls
    },
    // 删除指定dom元素的样式
    removeClass (ele, cls) {
      if (this.hasClass(ele, cls)) {
        var reg = new RegExp('(\\s|^)' + cls + '(\\s|$)')
        ele.className = ele.className.replace(reg, ' ')
      }
    },
    handleClick (row, column, cell, event) {
      if (this.hasClass(cell.getElementsByTagName('button')[0], 'active')) {
        this.removeClass(cell.getElementsByTagName('button')[0], 'active')
        return
      }
      var tableList = document.getElementsByClassName('el-button--text')
      for (let i = 0; i < tableList.length; i++) {
        if (this.hasClass(tableList[i], 'active')) {
          this.removeClass(tableList[i], 'active')
        }
      }
      this.addClass(cell.getElementsByTagName('button')[0], 'active')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.el-table{
  margin-left: 50px;
}
.el-table .active{
  padding-left: 30px;
  padding-right: 30px;
  background-color:blue;
  color: white;
}
.el-table__header-wrapper th{
  color: blue;
  font-size: 30px;
}
</style>
