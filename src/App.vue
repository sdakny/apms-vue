<template>
    <el-button type="primary" @click="handleLogin">test login</el-button>
    <el-button type="danger" @click="handleUserList">test user request</el-button>
</template>

<script setup>
import requestUtil from '@/utils/request'
import store from '@/store'

const handleLogin=async ()=>{
  let result = await requestUtil.get("test/login");
  let data = result.data;
  if(data.code = 200){
    const token = data.token;
    console.log("login success: token=" + token);
    store.commit('SET_TOKEN', token);
  }else{
    console.log("login error!");
  }
}

const handleUserList=async ()=>{
  let result = await requestUtil.get("test/user/list");
  let data = result.data;
  if(data.code = 200){
    const userList = data.userList;
    console.log("User list info: userList=" + userList);
  }
}

</script>

<style>

</style>
