<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios';
import { onMounted, ref } from 'vue';

// TODO: 列表渲染
// 思路： 声明响应式 list -> 调用接口获取数据 -> 后端数据赋值给 list -> 绑定到table组件
const list = ref([])

const getList = async () => {

  // 调用接口
  const res = await axios.get('/list')
  console.log(res);

  // 交给 list
  list.value = res.data
}

// 在 onMounted 生命周期阶段 才调用接口
onMounted(() => getList())


// TODO: 删除功能
// 思路： 获取 当前行的 id -> 通过 id 调用 删除接口 -> 更新 最新的列表

// 1、首先准备一个删除的回调函数，然后给做个点击事件绑定到 删除按钮 dom元素上，然后给 删除的回调函数 传个形参 id
const onDelete = async (id) => {

  console.log(id)

  // 2、通过 id 调用 删除接口 ，不需要用变量接收，通知一下用户成功了即可
  await axios.delete(`/del/${id}`)

  // 3、更新 最新的列表
  getList()
}


// TODO: 编辑功能
// 思路： 点击 编辑 来打开弹框 -> 弹框 中 回填数据 -> 更新数据

// 1、打开弹框  （获取子组件实例 调用方法 或者 修改属性）
const editRef = ref(null)  // 既然 生成的是对象 ，先用 null 占个位置

// 4、调用 子组件实例 中的 open（）方法   8、传参
const onEdit = (row) => {
  editRef.value.open(row)
}

// 6、 回填数据 （使用 调用详情接口 或者 当前行的 静态数据）
// 当前行的静态数据 在父组件中，而是 使用这个数据 在子组件中

</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }"> <!-- 5、点击事件  7、传参 -->
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <!-- 18.1 自定义事件： 子 传 父-->
  <Edit ref="editRef" @on-update="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
