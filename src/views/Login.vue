<template>
  <el-form ref="form" label-width="70px" class="longin-container" :inline="true" :model="form" :rules="rules"> 
    <h3 class="login_title">系统登录</h3>
    <el-form-item label="用户名" prop="username"> 
        <el-input v-model="form.username" placeholder="请输入账号"></el-input>
    </el-form-item>
    <el-form-item label="密码" prop="password"> 
        <el-input type="password" v-model="form.password" placeholder="请输入密码"></el-input>
    </el-form-item>
    <el-form-item style="margin-left:105px;margin-top:10px"> 
        <el-button @click="submit" type="primary">登录</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import Mock from 'mockjs'
import Cookie from 'js-cookie'
import {getMenu} from '../api'
export default {
    data(){
        return{
            form:{ 
                username:'',
                password:'',
            },
            rules:{ 
                username:[
                    {required:true,trigger:'blur',message:'请输入用户名'}
                ],password:[
                    {
                        required:true,trigger:'blur',message:'请输入密码'
                    }
                ]
            }
        }
    },
    methods:{
        // 登录
        submit(){
            // // token信息
            // const token=Mock.Random.guid()
            // // token信息存入cookie用于不同页面的通信
            // Cookie.set('token',token)
            // // 跳转到首页
            // this.$router.push('/home')

            // 校验通过
            this.$refs.form.validate((valid)=>{
                if(valid){
                    getMenu(this.form).then(({data})=>{
                        console.log(data)
                        if(data.code===20000){
                            // token信息存入cookie用于不同页面之间的通信
                          Cookie.set('token',data.data.token)

                        //   获取菜单的数据，存入store中
                       this.$store.commit('setMenu', data.data.menu)
                       this.$store.commit('addMenu',this.$router)
                       console.log('router',this.$router)
                        //   跳转到首页
                        this.$router.push('/home')
                        }else{
                            this.$message.error(data.data.message)
                        }
                    })
                }
            })
            
        }
    }
}
</script>

<style lang="less" scoped>
.longin-container{
    border:1px solid #eaeaea;
    width: 350px;
    margin:180px auto;
    padding:35px 35px 15px 35px;
    border-radius:15px ;
    box-shadow:0 0 25px #cac6c6; 
    box-sizing: border-box;
    .login_title{
      text-align: center;
        margin-bottom:40px;
    }
    .el-input{
        width:198px;
    }
}
</style>