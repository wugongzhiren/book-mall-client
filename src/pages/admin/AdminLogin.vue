<template>
  <div class="ClientLogin" :style="{width:width+'px',height:height+'px'}">
    <el-card class="content">
      <el-row>
        <img src="/src/assets/img/logo.png"/>
      </el-row>
      <div>


        <el-input v-model="account" type="text" placeholder="请输入账号" />
        <br>
        <el-input style="margin-top: 20px" v-model="pwd" type="password" placeholder="请输入密码" />
        <el-button style="margin-top: 20px" type="danger" @click="login">登录</el-button>
      </div>
    </el-card>
  </div>
</template>

<script>
  import {getClientSize} from '../../util/util';


  export default {
    name: 'AdminLogin',
    computed:{
      width(){
        return getClientSize().width;
      },
      height(){
        return getClientSize().height;
      },
    },
    data () {
      return {
        account:'',
        pwd:''
      }
    },
    methods:{
      login(){
        if(this.account=='admin'&&this.pwd=='admin'){
          this.$router.push('/backstage');
        }else{
          alert('登录失败')
        }

        /* const account = this.$refs.account.value;
         const pwd = this.$refs.pwd.value;
         const res = login({
           account:account,
           pwd:pwd
         });
         res
         .then((data)=>{

           this.$router.push('/backstage');
         })
         .catch((e)=>{
           alert('登录失败')
         })*/
      }
    }
  }
</script>

<style scoped lang="less">
  @import "../../assets/css/var.less";
  .ClientLogin{
    background-color: #F9F9F9;
    position: relative;
    .content{
      background-color: #FCFCFC;
      width: 350px;
      position: absolute;
      top: 50%;
      left: 50%;
      margin-top: -260px;
      margin-left: -150px;
      text-align: center;
      overflow: hidden;
      h3{
        color:@secondColor;
        font-size: 18px;
      }
      .tag{
        margin-top: 20px;
        color:@fontDefaultColor;
        margin-bottom: 20px;
        span{
          display: inline-block;
          width: 50px;
          text-align: center;
          margin: 0 10px;
          padding: 10px 0;
          cursor: pointer;
        }
        /* .selected{
           border-bottom: 2px solid @secondColor;
           color:@secondColor
         }*/
      }
      input{
        border-radius: 0;
        box-shadow: none;
        background: #fff;
        padding: 14px;
        width: 80%;
        border: 1px solid @borderColor;
        margin-bottom: 10px;
      }
      /*button{
        width: 100%;

        box-shadow: none;
        border: 0;
        border-radius: 3px;
        line-height: 41px;
        color: #fff;
        cursor: pointer;
        margin-top: 20px;
      }*/
    }
  }
</style>
