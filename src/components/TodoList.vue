<script setup>
import {ref, watch} from "vue";
import {ElNotification} from "element-plus";

const localStageName = 'todo-localStage';
let info = ref('');
let list = ref(localStorage.getItem(localStageName) == null ? [] : JSON.parse(localStorage.getItem(localStageName)));

function addItem() {
  if (info.value !== '') {
    list.value.push({"value": info.value, "checked": false});
    info.value = '';
  } else {
    alert('输入框不得为空')
  }
}

function deleteItem(item) {
  list.value = list.value.filter((t) => t !== item)

  return ElNotification({
    title: 'Success',
    message: 'Delete Success',
    type: 'success',
  })
}

watch(list.value, (newValue, _) => {
  console.log("123")
  localStorage.setItem(localStageName, JSON.stringify(list.value))
})

watch(list, (newValue, _) => {
  console.log("456")
  localStorage.setItem(localStageName, JSON.stringify(list.value))
})
</script>

<template>
  <div class="body">
    <h1>TODO-LIST</h1>
    <div>
      <input type="text" v-model="info" @keyup.enter="addItem">
      <button @click="addItem">添加</button>
    </div>
    <ul>
      <li class="item" v-for="item in list">
        <input type="checkbox" v-model="item.checked">
        <span :class="{finish: item.checked}">{{ item.value }}</span>
        <el-button plain @click="deleteItem(item)">X</el-button>
      </li>
    </ul>
  </div>
</template>

<style scoped>

h1 {
  color: #6499E9;
}

.body {
  background-color: antiquewhite;
  display: flex;
  flex-flow: column wrap;
  align-items: center;
  margin: 20px 20px;
  border: red dashed;
}

.finish {
  text-decoration: line-through;
}

</style>