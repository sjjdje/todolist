<template>
  <div id="app">
    <el-row>
      <el-col :span="18">
        <el-input v-model="input" placeholder="请输入任务"></el-input>
      </el-col>
      <el-col :span="6">
        <el-button class="searchBtn" type="primary" @click="addTask">添加事项</el-button>
      </el-col>
    </el-row>
    <el-card class="box-card" shadow="always">
      <el-row v-for="(item,index) in task" :key="index">
        <el-col :span="16">
          <el-checkbox v-model="item.complete" @change="changTaskStatus">{{item.taskName}}</el-checkbox>
        </el-col>
        <el-col class="remove" :span="4">
          <a href="javascript:;" @click="removeTask(index)">删除</a>
        </el-col>
        <el-col class="remove" :span="4">
          <a href="javascript:;" @click="editTask(index)">编辑</a>
        </el-col>
      </el-row>
    </el-card>
    <div class="info">
      <el-button type="primary" @click="allTask">全部</el-button>
      <el-button type="primary" @click="notDoneTask">未完成</el-button>
    </div>
    <el-dialog title="编辑任务" :visible.sync="dialogVisible" width="30%">
      <el-input v-model="editinput" placeholder="请输入新任务"></el-input>
      <span slot="footer" class="dialog-footer">
        <el-button @click="cancleEdit">取 消</el-button>
        <el-button type="primary" @click="editSure(index)">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      // 存储表单的值
      input: '',
      editinput: '',
      // 存储任务事项
      task: [],
      // 监听编辑对话框的显示与影藏
      dialogVisible: false,
      // 存储编辑按钮点击的数组索引
      index: 0
    }
  },
  created() {
    this.getTaskData()
  },
  methods: {
    // 监听添加事项按钮点击事件
    addTask() {
      if (this.input.trim().length > 0) {
        this.task.push({ taskName: this.input, complete: false })
        this.setLocalStorage()
      } else {
        alert('请输入有效任务')
      }
      this.input = ''
    },
    // 为task数组赋值
    getTaskData() {
      const task1 = localStorage.getItem('todo')
      this.task = JSON.parse(task1)
      // 如果删除todo会使task变为null，这里做下处理将其变为数组
      if (this.task === null) {
        this.task = []
      }
    },
    // 监听任务状态变化事件
    changTaskStatus() {
      // console.log(this.task)
      this.setLocalStorage()
    },
    // 封装设置LocalStorage方法
    setLocalStorage() {
      localStorage.setItem('todo', JSON.stringify(this.task))
    },
    // 监听删除按钮点击事件
    removeTask(index) {
      // console.log(index)
      this.task.splice(index, 1)
      // console.log(this.task)
      this.setLocalStorage()
    },
    // 监听编辑按钮点击事件
    editTask(index) {
      // console.log(index)
      this.index = index
      this.dialogVisible = true
      // console.log(this.index)
    },
    // 监听编辑对话框确定事件
    editSure() {
      if (this.editinput.trim().length > 0) {
        this.task[this.index].taskName = this.editinput
        this.setLocalStorage()
        this.editinput = ''
        this.dialogVisible = false
      } else {
        alert('请输入有效任务')
      }
    },
    // 监听编辑对话框取消事件
    cancleEdit() {
      this.editinput = ''
      this.dialogVisible = false
    },
    // 监听未完成按钮点击事件
    notDoneTask() {
      const newTask = this.task.filter(item => {
        return item.complete === false
      })
      // console.log(newTask)
      this.task = newTask
    },
    // 监听全部按钮点击事件
    allTask() {
      const newTask = localStorage.getItem('todo')
      if (newTask !== null) {
        // console.log(11)
        // newTask = JSON.parse(newTask)
        // console.log(jsnewTask)
        const newTask1 = JSON.parse(newTask)
        // this.task = newTask
        // console.log(newTask1)
        this.task = newTask1
      } else {
        alert('您目前没有任务')
      }
    }
  }
}
</script>

<style lang="less" scoped>
#app {
  margin: 0 auto;
  width: 500px;
  .searchBtn {
    margin-left: 10px;
  }
  .box-card {
    margin-top: 10px;
    .el-checkbox {
      position: relative;
      width: 100%;
      padding: 10px 0;
    }
    .remove {
      line-height: 35px;
      a {
        text-decoration: none;
      }
    }
  }
  .info {
    margin-top: 15px;
    .el-button {
      margin-left: 5px;
    }
  }
}
</style>
