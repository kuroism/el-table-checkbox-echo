<template>
  <div id="app">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>el-table checkbox 回显解决方案</span>
      </div>
      <span>multipleSelection: {{ multipleSelection }}</span>
      <el-table
        ref="multipleTable"
        :data="curTableData"
        tooltip-effect="dark"
        style="width: 100%"
        @selection-change="handleSelectionChange"
        :row-key="(row) => { return row.id }">
        <el-table-column
          type="selection"
          :reserve-selection="true"
          width="55">
        </el-table-column>
        <el-table-column
          label="日期"
          width="120">
          <template slot-scope="scope">{{ scope.row.date }}</template>
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="120">
        </el-table-column>
        <el-table-column
          prop="address"
          label="地址"
          show-overflow-tooltip>
        </el-table-column>
      </el-table>
      <el-pagination
        background
        :current-page.sync="currentPage"
        :page-size="5"
        layout="prev, pager, next"
        :total="tableData.length">
      </el-pagination>
    </el-card>

    <el-divider></el-divider>

    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>el-table checkbox 回显解决方案 2</span>
      </div>
      <span>selected : {{ selected }}</span>
      <el-table
        ref="multipleTable2"
        :data="curTableData"
        tooltip-effect="dark"
        style="width: 100%"
        @select="handleSelect"
        @select-all="handleSelectionAll">
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          label="日期"
          width="120">
          <template slot-scope="scope">{{ scope.row.date }}</template>
        </el-table-column>
        <el-table-column
          prop="name"
          label="姓名"
          width="120">
        </el-table-column>
        <el-table-column
          prop="address"
          label="地址"
          show-overflow-tooltip>
        </el-table-column>
      </el-table>
      <el-pagination
        background
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage"
        :page-size="5"
        layout="prev, pager, next"
        :total="tableData.length">
      </el-pagination>
    </el-card>
    
  </div>
</template>

<script>


export default {
  name: 'app',
  components: {
  },
  data() {
    return {
      tableData: [{
        id: 1,
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 2,
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 3,
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 4,
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 5,
        date: '2016-05-05',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 6,
        date: '2016-05-06',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        id: 7,
        date: '2016-05-07',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }],
      multipleSelection: [],
      currentPage: 1,
      selected: []
    };
  },
  computed: {
    // 前端分页
    curTableData() {
      return this.tableData.slice((this.currentPage - 1) * 5, this.currentPage * 5)
    }
  },
  methods: {
    // 解决方案 1 
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },

    // 解决方案 2 
    // 回显
    echo() {
      let rows = []
      this.curTableData.forEach(row => {
        this.selected.forEach(item => {
          if (row.id === item.id) {
            rows.push(row)
          }
        })
      })
      this.toggleSelection(rows)
    },
    // 回显
    toggleSelection(rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable2.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable2.clearSelection();
      }
    },
    // 从selected list中去除某项
    handleDelItem(row){
      let index = -1
      this.selected.forEach((item, idx) => {
        if(item.id === row.id) {
          index = idx
        }
      })
      if (index >= 0) {
        this.selected.splice(index, 1)
      }
    },
    // 单选
    handleSelect(selection, row) {
      let isExited = false 
      this.selected.forEach(item => {
        if(item.id === row.id) {
          isExited = true
        }
      })
      if (isExited) {
        this.handleDelItem(row)
      } else {
        this.selected.push(row)
      }
    },
    // 全选 or 全不选
    handleSelectionAll(selection) {
      if (selection.length === 0) {
        this.curTableData.forEach(item => {
          this.handleDelItem(item)
        })
      } else {
        this.selected = this.unique(
          this.selected.concat(this.curTableData)
        )
      }
    },
    // 数组去重
    unique(arr) {
      return [...new Set(arr)]
    },
    // 当前页面切换触发回显
    handleCurrentChange() {
      this.echo()
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
.box-card {
  width: 80%;
  margin: 0 auto;
}
</style>
