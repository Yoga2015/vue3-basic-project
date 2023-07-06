<script setup>
// TODO: 编辑
import { ref } from 'vue'

import axios from 'axios'

// 弹框开关
const dialogVisible = ref(false)

// 11、16.1 、那得先准备一个form响应式对象，未来用来装 父组件 传递过来的 数据
const form = ref({
  name: '',
  place: '',
  id: ''
})

// 2、设置 显示弹框   9、数据是传过来了，但是最终想绑定在表单上
const open = (row) => {

  console.log(row);   //10、打印看看 父组件 传递过来的 '当前行的静态数据' 

  // 12、16.2 赋值给 响应式对象 的 属性
  form.value.name = row.name
  form.value.place = row.place
  form.value.id = row.id
  dialogVisible.value = true
}

// 3、使用 defineExpose() 编译宏函数  指定 open 方法 允许访问
defineExpose({
  open
})

// 14、更新数据  18.2、通过defineEmits() 编译器宏函数 生成 emit()方法
const emit = defineEmits(['on-update'])

const onUpdate = async () => {

  // 16、收集表单数据 调用接口
  await axios.patch(`/edit/${form.value.id}`, {
    name: form.value.name,
    place: form.value.place,
  })
  // 17、关闭弹框
  dialogVisible.value = false

  // 18、通知 父组件 做 列表更新  (子 传 父)
  emit('on-update')

}
</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form label-width="50px">
      <el-form-item label="姓名"> <!--13、绑定响应式对象的属性-->
        <el-input placeholder="请输入姓名" v-model="form.name" />
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input placeholder="请输入籍贯" v-model="form.place" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <!-- 15、关闭弹框 -->
        <el-button type="primary" @click="onUpdate">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
