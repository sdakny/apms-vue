<template>
  <div class="login">
  
    <el-form ref="loginRef" :model="loginForm" :rules="loginRules" class="login-form">
      <h3 class="title">APMS</h3>

      <el-form-item prop="username">

        <el-input
            v-model="loginForm.username"
            type="text"
            size="large"
            auto-complete="off"
            placeholder="Username"
        >
        <template #prefix><svg-icon icon="user" /></template>
        </el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
            v-model="loginForm.password"
            type="password"
            size="large"
            auto-complete="off"
            placeholder="Password"
            @keyup.enter="handleLogin"
        >
        <template #prefix><svg-icon icon="password" /></template>
        </el-input>
      </el-form-item>


      <el-checkbox  style="margin:0px 0px 25px 0px;">Remember password</el-checkbox>
      <el-form-item style="width:100%;">
        <el-button
            size="large"
            type="primary"
            style="width:100%;"
            @click.prevent="handleLogin"
        >
          <span>Login</span>

        </el-button>

      </el-form-item>
    </el-form>
    <!--  底部  -->
    <div class="el-login-footer">
      <span>Github link: -- <a href="https://github.com/sdakny/" target="_blank">sdakny</a> -- 2023</span>
    </div>
  </div>
</template>

<script setup>
  import requestUtil from '@/utils/request'
  import store from '@/store'
  import {ref} from 'vue'
  import qs from 'qs'
  import { ElMessage } from 'element-plus'

  const loginRef = ref(null)

  const loginForm=ref({
    username:"",
    password:""
  })

  const loginRules = {
    username: [{ required: true, trigger: "blur", message: "Please input username" }],
    password: [{ required: true, trigger: "blur", message: "Please input password" }]
  };

  const handleLogin=()=>{
    loginRef.value.validate(async (valid)=>{
      if(valid){
        try{
            let result=await requestUtil.post("login?"+qs.stringify(loginForm.value))
            let data=result.data;
            if(data.code==200){
              const token = data.authorization;
              store.commit('SET_TOKEN',token);
            }else{
              ElMessage.error(data.msg)
            }
        }catch(err){
          console.log("error:"+error);
          ElMessage.error("Server error, please contact the administrator!")
        }
      }else{
        console.log("Validate fail")
      }
    })
  }

</script>

<style lang="scss" scoped>
a{
  color:white
}
.login {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background-image: url("../assets/images/login-background.jpg");
  background-size: cover;
}
.title {
  margin: 0px auto 30px auto;
  text-align: center;
  color: #707070;
}

.login-form {
  border-radius: 6px;
  background: #ffffff;
  width: 400px;
  padding: 25px 25px 5px 25px;

  .el-input {
    height: 40px;



    input {
      display: inline-block;
      height: 40px;
    }
  }
  .input-icon {
    height: 39px;
    width: 14px;
    margin-left: 0px;
  }

}
.login-tip {
  font-size: 13px;
  text-align: center;
  color: #bfbfbf;
}
.login-code {
  width: 33%;
  height: 40px;
  float: right;
  img {
    cursor: pointer;
    vertical-align: middle;
  }
}
.el-login-footer {
  height: 40px;
  line-height: 40px;
  position: fixed;
  bottom: 0;
  width: 100%;
  text-align: center;
  color: #fff;
  font-family: Arial;
  font-size: 12px;
  letter-spacing: 1px;
}
.login-code-img {
  height: 40px;
  padding-left: 12px;
}
</style>