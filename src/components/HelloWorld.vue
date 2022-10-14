<script setup>
import { ref, watch, reactive } from "vue";

defineProps({
  msg: String,
});

const count = ref(2);
const type = ref("");
const nameString = ref("");
const nameArr = reactive({
  list: [],
});
const resultArr = reactive({
  list: [],
});
const options = [
  {
    value: 1,
    label: "手动输入",
  },
  {
    value: 2,
    label: "导入表格",
  },
];
watch(nameString, (val) => {
  if (val) {
    const arr = val.split(/[(\r\n)|(\s)]+/);
    arr.forEach((item, index) => {
      if (!item) {
        arr.splice(index, 1); //删除空项
      }
    });
    nameArr.list = arr;
  }
});
const startVote = () => {
  const resSet = new Set();
  const len = nameArr.list.length;
  while (resSet.size < count.value) {
    // 均衡的产生0 - len-1 之间的随机数
    const index = Math.floor(Math.random() * len);
    resSet.add(nameArr.list[index]);
  }
  resultArr.list = Array.from(resSet);
};
const handleClose = (index) => {
  nameArr.list.splice(index, 1)
}
</script>

<template>
  <h1>{{ msg }}</h1>
  <div class="input-container">
    <span class="label">取餐人员数目:</span>
    <el-input class="flex-1" v-model="count" placeholder="请填写除刘航外还需要几位取餐人员" clearable />
  </div>
  <div class="input-container">
    <span class="label">人员生成方式:</span>
    <el-select class="flex-1" v-model="type" placeholder="请选择生成方式">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
    </el-select>
  </div>
  <div v-if="type===1" class="input-container">
    <span class="label">手动输入人名:</span>
    <el-input class="flex-1" v-model="nameString" :rows="4" type="textarea" placeholder="请输入人员名单" />
  </div>
  <div v-if="nameArr.list.length" class="input-container">
    <span class="label">候选人员名单:</span>
    <div class="felx-1 name-container">
      <el-tag class="name-tag" closable @close="handleClose(index)"  v-for="(item, index) in nameArr.list" :key="`name${index}`">{{item}}</el-tag>
    </div>
  </div>
  <div v-if="nameArr.list.length" class="input-container button-container">
    <el-button type="success" @click="startVote">开始公平选举</el-button>
  </div>
  <div v-if="resultArr.list.length" class="input-container">
    <span class="label">取餐人员名单:</span>
    <div class="felx-1 name-container">
      <el-tag
        class="name-tag"
        v-for="(item, index) in resultArr.list"
        :key="`name${index}`"
      >{{item}}</el-tag>
    </div>
  </div>
</template>

<style lang="less" scoped>
a {
  color: #42b983;
}
.input-container {
  margin-top: 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 400px;
  margin-left: calc(50% - 200px);
  .label {
    text-align: left;
    min-width: 120px;
  }
  .flex-1 {
    flex-grow: 1;
  }
  .name-container {
    border: 1px solid #dfdce3;
    width: 300px;
    height: 100px;
    overflow-y: auto;
    .name-tag {
      margin: 5px;
    }
  }
}
.button-container {
  justify-content: center;
}
</style>
