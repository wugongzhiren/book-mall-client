<template>
  <div class="ClientLogin" :style="{width:width+'px',height:height+'px'}">
    <div class="content">
      <div v-show="curIndex===0">

        <el-input v-model="userid" type="text" placeholder="请输入账号" />
        <br>
        <el-input style="margin-top: 20px" v-model="signPwd" type="password" placeholder="请输入密码" />
        <el-button style="margin-top: 20px" type="danger" @click="login">登录</el-button>
        <el-button type="danger"  @click="setIndex(1)">注册</el-button>
      </div>
      <div class="formBox" v-show="curIndex===1">
        <el-input style="margin-top: 20px" v-model="userid" type="text" placeholder="请输入邮箱或者手机号" />
        <el-input style="margin-top: 20px" v-model="signPwd" type="password" placeholder="请输入密码" />
        <el-input style="margin-top: 20px" v-model="signName" type="text" placeholder="请输入收件人姓名" />
        <el-input style="margin-top: 20px" v-model="phone" type="text" placeholder="请输入联系电话" />
        <el-input style="margin-top: 20px" v-model="signAddress" type="text" placeholder="请输入收件地址" />
        <el-button style="margin-top: 20px" type="danger"  @click="signup">注册</el-button>
        <el-button type="danger" @click="setIndex(0)">返回登陆</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'
import {getClientSize} from '../../util/util';
import {login,signup} from '../../api/client';

export default {
  name: 'ClientLogin',
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
      curIndex:0,
      userid:"",
      signPwd:"",
      signName:"",
      signAddress:"",
      phone:''
    }
  },
  methods:{
    ...mapMutations({
     // setClientName: 'SET_CLIENT_NAME',
      setClientToken: 'SET_CLIENT_TOKEN'
    }),
    setIndex(index){
      if(index===this.curIndex){
        return;
      }
      this.curIndex = index;
    },
    login(){
      if(this.userid==''||this.signPwd==''){
        this.$message('请输入完整信息');
        return;
      }
      const res = login({
        userid:this.userid,
        pwd:this.signPwd
      });
      res
      .then((data)=>{
        if(data.code==200){
          this.setClientToken(data.t.userid);
          this.$router.push('/');
        }else{
          alert("账号或者密码错误，请重试")
        }

      })
      .catch((e)=>{
        alert(e)
      })
    },
    signup(){
      if(this.userid==''||this.signName==''||this.signPwd==''||this.signAddress==''||this.phone==''){
        this.$message('请输入完整信息')
        return;
      }
      const res = signup({
        userid:this.userid,
        username:this.signName,
        pwd:this.signPwd,
        address:this.signAddress,
        phone:this.phone
      });
      res
      .then((data)=>{
        if(data.code==201){
          this.$message("该用户已经被注册!");
        }else{
          this.curIndex=0;
          this.$message("注册成功，欢迎登陆!");
        }
      })
      .catch((e)=>{
        this.$message('出错了')
      })
    }
  }
}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
.ClientLogin{
  background-color: #eee;
  position: relative;
  .content{
    width: 300px;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -260px;
    margin-left: -150px;
    text-align: center;
    overflow: hidden;
    h3{
      color:@secondColor;
      font-size: 50px;
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
