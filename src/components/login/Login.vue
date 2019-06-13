<template>
<div class="login-wrapper">
  <el-row type="flex" class="loginForm" justify="center" align="middle">
  <el-col :xs="12" :sm='8' :md="8" :lg="6" :xl="1" class="login-content">
  <el-form label-position="top" :model="loginForm" :rules="rules" ref="loginForm" label-width="100px" >
  <el-form-item label="用户名" prop="username">
    <el-input v-model="loginForm.username"></el-input>
  </el-form-item>
  <el-form-item label="密码" prop="password">
    <el-input type="password" v-model="loginForm.password"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="submitForm()">登录</el-button>
    <el-button @click="resetForm()">重置</el-button>
  </el-form-item>
</el-form>
</el-col>
</el-row>
</div>

</template>
<script>
import axios from 'axios'
export default {
  data() {
      return {
        loginForm: {
          username: '',
          password:''
        },
        rules: {
          username: [
            { required: true, message: '用户名不能为空', trigger: 'blur' },
            { min: 3, max: 6, message: '长度在 3 到 6 个字符', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' },
            { min: 3, max: 6, message: '长度在 3 到 6 个字符', trigger: 'blur' }
          ],
        }
      };
    },
    methods: {
      // 登录功能实现
      login(){
        // http://localhost:8888/api/private/v1/login
        axios.post('http://localhost:8888/api/private/v1/login',this.loginForm)
        .then(res=>{
          console.log(res);
        const {data,meta}=res.data
        if(meta.status==200){
          console.log('登录成功');
          this.$router.push('/home')
        }else{
          console.log('登录失败',meta.msg)
          this.$message.error(meta.msg)
        }
        })
      },
      submitForm() {
        this.$refs.loginForm.validate((valid) => {
          if (valid) {
        console.log(this.loginForm);
        this.login();

          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm() {
     this.$refs.loginForm.resetFields();
      }
    }
}
</script>

<style>
.login-wrapper,.loginForm{
  height: 100%;
}
.loginForm{
  background-color: #2D434C;
}
.login-content{
  background: #fff;
  min-width: 240px;
  padding: 20px 35px;
  border-radius: 10px;
}
</style>
