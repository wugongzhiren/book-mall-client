<template>
  <div class="Backstage" :style="{width:width+'px',height:height+'px'}">
    <div class="bsLeft">
      <!--<div class="logo">XXXの蛋糕店</div>-->
      <ul :style="{height:(height-70)+'px'}">
        <li @click="navTo('/backstage/editUser')" :class="{selected:curPath==='/backstage/editUser'}">用户管理</li>
        <li @click="navTo('/backstage/goods')" :class="{selected:curPath==='/backstage/goods'}">图书管理</li>
        <li @click="navTo('/backstage/orders')" :class="{selected:curPath==='/backstage/orders'}">订单管理</li>
        <li @click="navTo('/backstage/editAds')" :class="{selected:curPath==='/backstage/editAds'}">公告管理</li>
        <li @click="navTo('/backstage/suggest')" :class="{selected:curPath==='/backstage/suggest'}">数据维护</li>
      </ul>
    </div><div class="bsRight">
      <div class="header">
        <div class="title">后台管理系统</div>
      </div>
      <transition name="router-fade" mode="out-in">
          <router-view :style="{height:(height-70)+'px',overflowY:'scroll',width:'100%',padding:'20px',backgroundColor:'#f6f5fa'}"></router-view>
      </transition>
    </div>
  </div>
</template>

<script>
import { mapState,mapMutations } from 'vuex';
import {getClientSize} from '../../util/util';
export default {
  name: 'Backstage',
  data(){
    return{
      userTipsShow:false,
      curPath:this.$route.path
    }
  },
  computed:{
    ...mapState([
      'adminName'
    ]),
    width(){
      return getClientSize().width;
    },
    height(){
      return getClientSize().height;
    },
  },
  methods:{
    ...mapMutations({
      adminLogout: 'ADMIN_LOGOUT',
    }),
    showUserTips(){
      this.userTipsShow = true;
    },
    closeUserTips(){
      this.userTipsShow = false;
    },
    navTo(route){
      if(this.curPath===route){
        return;
      }
      this.$router.push(route)
    },
    logout(){
      this.adminLogout();
      this.$router.push('/login');
    }
  },
  watch:{
    '$route'(to,from){
        this.curPath = to.path;
    }
  }
}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
.Backstage{
  overflow: hidden;
  .bsLeft{
    font-size: 18px;
    width: 15%;
    color: white;
    background-color: #ff5074;
    height: 100%;
    display: inline-block;
    overflow: hidden;
    user-select:none;
    .logo{
      width: 100%;
      height: 70px;
      background-color: #ff5074;
      color:white;
      font-size: 25px;
      overflow: hidden;
      text-align: center;
      line-height: 70px;
    }
    ul{
      width: 100%;
      background-color: #ff5074;
      li{
        width: 100%;
        color:white;
        height: 45px;
        line-height: 45px;
        cursor: pointer;
        padding: 0 20px;
      }
      .selected{
        background-color: #ff260b;
        color:white;
      }
    }
  }
  .bsRight{
    width: 85%;
    height: 100%;
    display: inline-block;
    overflow: hidden;
    .header{
      width: 100%;
      height: 70px;
      line-height: 70px;
      border-bottom: 1px solid @borderColor;
      font-size: 18px;
      position: relative;
      user-select:none;
      .title{
        font-size: 23px;
        position: absolute;
        left: 10px;
        vertical-align: middle;
        color:@fontDefaultColor;
      }
      .userInfo{
        position: absolute;
        right: 10px;
        font-size: 14px;
        i{

        }
        span{
          cursor: pointer;
        }
        .userTips{
          position: absolute;
          top: 45px;
          right: 0;
          width: 66px;
          cursor: pointer;
          border: 1px solid @borderColor;
          background-color: white;
          li{
            width: 100%;
            height: 30px;
            text-align: center;
            line-height: 30px;
            border-bottom: 1px solid @borderColor;
          }
        }
      }
    }
  }
}
</style>
