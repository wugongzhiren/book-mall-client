<template>
  <div class="Mall">

    <el-row v-if="adimg!=''" type="flex" justify="center">
      <img v-if="adimg!=''" style="width: 100%;height: 100%;align:middle;position: relative;
" :src="adimg" alt=""/>
    </el-row>
    <el-row>
    <header>
      <div class="container clear">
        <!--<div  v-if="clientToken">
          <span class="name">欢迎您</span>
          <span @click="navTo('/mall/personal')">个人中心</span>
          <span @click="logout">退出登录</span>

        </div>-->

          <span class="title" @click="navTo('/mall')"> 欢迎<span v-if="clientToken" >光临</span><span v-else style="color: red" @click.stop="navTo('/login')">登录</span>字里行间网上书店</span>
        <!--<i class="iconfont icon-search" @click="searchConfirm"/>
        <input  class="TextInput" v-model="searchText" placeholder="请输入商品关键字"/>-->

        <div style="margin-left: 400px;">
          <van-search
            v-model="searchText"
            show-action
            style="width: 400px;"
            placeholder="请输入书名、作者或者类别"
          >
            <div slot="action" @click="searchConfirm">搜索</div>
          </van-search>
         <!-- <van-search v-model="value" style="width: 400px;" background=" #F1F1F1" placeholder="请输入搜索关键词" >

          </van-search>-->
        </div>
        <!--<div class="right" v-if="clientToken">
          <span class="name">欢迎您</span>
          <span @click="navTo('/mall/personal')">个人中心</span>
          <span @click="logout">退出登录</span>
        </div>
        <div class="right" v-else>

        </div>-->

        <div class="right" > <el-button type="danger" @click="navTo('/mall/personal/cart')" size="small"  icon="el-icon-shopping-cart-full">购物车</el-button><el-button type="danger"  @click="navTo('/mall/personal')" size="small" icon="el-icon-user">个人中心</el-button><el-button type="danger"  @click="logout" size="small">退出登录</el-button></div>
      </div>
    </header>

    <div class="content" :style="{minHeight:clientHeight+'px'}">
      <div class="container">
        <router-view></router-view>
      </div>
    </div>
    <div class="bottomInfo">
      <div class="container">
        <div class="service footerItem">
          <p class="title">客户服务</p>
          <span><i class="iconfont icon-people_fill" />在线客服</span>
          <span><i class="iconfont icon-fabulous" />用户反馈</span>
        </div>
        <div class="intro footerItem">
          <p class="title">字里行间网上书店</p>
          <p class="intro-p">世间财富万万千，唯有诗书最宝贵。钻石比之稍逊色，金银更是不能言。遍览群书似富翁，一生尽得他人尊；</p>
          <div>
            关注我们：
            <img src="http://yanxuan.nosdn.127.net/60068701f3a380911f237c26c91b39d0.png" alt=""/>
            <img src="http://yanxuan.nosdn.127.net/031e783d7ee645b6096980d0bf83079b.png" alt=""/>
            <img src="http://yanxuan.nosdn.127.net/0c8759a89cdbd7acf7f2921e6f0fad19.png" alt=""/>
          </div>
        </div>
        <div class="code footerItem">
          <p class="title">欢迎关注微信公众号</p>
          <div id="qrcode"><img style="width: 100px" src="/src/assets/img/QR.jpg"></div>
        </div>
      </div>
    </div>
    <footer>
      <div class="container">

        <div class="footerBottom">

          <ul>
            <li>Copyright2020 ©XXXの字里行间 版权所有</li>
          </ul>
          <p>XXX公司版权所有 © 1996-2020  许可证：XXXXXXXXXXXXXXXXX</p>
          <ul style="margin-top: 10px">

            <span style="cursor:pointer;" @click="See('https://www.taobao.com')">淘宝网 |</span>
            <span style="cursor:pointer;" @click="See('http://www.dangdang.com/')">当当网 |</span>
            <span style="cursor:pointer;" @click="See('http://www.jd.com/')">京东商城 </span>
          </ul>
        </div>
      </div>
    </footer>
    </el-row>
  </div>

</template>
<script>
  const os = require('os');
  import QRCode  from "qrcodejs2"
import { mapState,mapMutations } from 'vuex';
import NoticeList from '../../components/NoticeList';
import {getClientSize,backToTop} from '../../util/util';
import {getAds,getIp} from "../../api/client";
export default {
  name: 'Mall',
  computed:{
    ...mapState([
      'clientToken',
      'clientName'
    ]),
  },
  components:{
    NoticeList,
    QRCode
  },
  data () {
    return {
      adimg:'http://img63.ddimg.cn/2020/2/10/2020021018203860111.jpg',
      logo:'http://img61.ddimg.cn/upload_img/00405/luyi/DDlogoNEW.gif',
      notices:[],
      clientHeight:getClientSize().height,
      shouldShowBT:false,
      searchText:'',
      link: 'https://baidu.com'
    }
  },

  methods:{
    ...mapMutations({
      clientLogout: 'CLIENT_LOGOUT',
    }),
    See (e) {
      window.location.href = e
    },
    searchConfirm(){
      if(this.searchText.trim().length<=0){
        this.$message('输入不能为空！');
        return;
      }else{
        var text=this.searchText;
        this.searchText='';
        this.navTo(`/mall/show/goodsList/-1/${text}`);

      }

    },
    navTo(route){
      this.$router.push(route)
    },
    logout(){
      this.clientLogout();
      this.$router.go(0);
    },
    backToTop(){
      backToTop();
    },
   /* qrcode() {
      //this.getIPS();
      const res =  getIp();
      res
        .then((data) => {
          //alert(data.msg);
          let that = this;
          let qrcode = new QRCode('qrcode', {
            width: 124,
            height: 124,        // 高度
            text:  data.msg+":8080",   // 二维码内容
            // render: 'canvas' ,   // 设置渲染方式（有两种方式 table和canvas，默认是canvas）
            // background: '#f0f',   // 背景色
            // foreground: '#ff0'    // 前景色
          })
        })
        .catch((e) => {
          alert(e);
        });*/



    watchScrollTop(){
      let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
      if(scrollTop>150){
        this.shouldShowBT = true;
      }else{
        this.shouldShowBT = false;
      }
    }
  },

  created() {
   /* const res = getAds();
    res
      .then((data) => {
        if(data.t.length>0) {
          this.notices.push(data.t[0].tips1);
          this.notices.push(data.t[0].tips2);
          this.notices.push(data.t[0].tips3);

        }
      })
      .catch((e) => {
        alert(e);
      })*/
  },
  mounted(){
    document.addEventListener('scroll',this.watchScrollTop,false);
    //this.qrcode();
    const res = getAds();
    res
      .then((data) => {
        if(data.t.length>0) {
          //this.id = data.t[0].id;
          //this.tips = data.t[0].tips;
          this.adimg = data.t[0].img;
        }
      })
      .catch((e) => {
        alert(e);
      })
  },

  beforeDestroyed(){
    document.removeEventListener('scroll',this.watchScrollTop,false);
  }
}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
#qrcode {
  display: inline-block;
  /*img {
    width: 132px;
    height: 132px;
    background-color: #fff; //设置白色背景色
    padding: 6px; // 利用padding的特性，挤出白边
  }*/
}
.TextInput{
  float: right;
  border: 1px solid @borderColor;
  padding: 6px 10px;
  border-radius: 5px;
  display: inline-block;
  vertical-align: middle;
}
.icon-search{
  float: right;
  margin-right: 250px;
  font-size: 24px;
  font-weight: bold;
  color:white;
  cursor: pointer;
  position: relative;
  top: 4px;
}

.Mall{
  height: 1400px;
 /* width: 100%;*/
  header{
    width: 100%;
    background-color: #F9F9F9;
    height:68px;
    color:#646464;
    user-select:none;
    z-index: 10000;
    /*position: absolute;*/
    .container{
      position: relative;
      height:38px;
      .title{
        position: absolute;
        left: 0;
        display: inline-block;
        height: 26px;
        top: 50%;
        margin-top: 0px;
        line-height: 26px;
        font-size: 14px;
        cursor: pointer;
      }
      .right{
        position: absolute;
        right: 0;
        display: inline-block;
        height: 26px;
        top: 50%;
        margin-top: 0px;
        line-height: 26px;
        font-size: 14px;
        span{
          margin-left: 20px;
          cursor: pointer;
        }
        .name{
          cursor: default;
        }
      }
    }
  }
  .content{
    padding-top: 10px;
  }
  .fixedAd{
    position: fixed;
    right: 0;
    top: 608px;
    width: 72px;
    img{
      display: block;
      width: 100%;
      height: 154px;
    }
    .fixedList{
      margin-top: 2px;
      background-color: white;
      width: 100%;
      li{
        width: 100%;
        height: 80px;
        text-align: center;
        border-bottom: 1px solid @borderColor;
        cursor: pointer;
        padding-top: 12px;
        i{
          display: block;
          font-size: 30px;
          color:#666666;
        }
        span{
          display: block;
          font-size: 14px;
          color:#666666;
          margin-top: 4px;
        }
        &:last-child{
          border-bottom: none;
        }
        &:hover{
          i{
            color:@thirdColor;
          }
          span{
            color:@thirdColor;
          }
        }
      }
    }
  }
  .bottomInfo{
    width: 100%;
    height: 300px;
    border-top: 1px solid @borderColor;
    overflow: hidden;
    margin-top: 80px;
    .footerItem{
      width: 30%;
      height: 210px;
      position: relative;
      top: 45px;
      display: inline-block;
      text-align: center;
      vertical-align: middle;
      color:@fontDefaultColor;
      .title{
        color: @fontDeepColor;
        margin-bottom: 30px;
      }
    }
    .service{
      font-size: 18px;
      border-right: 1px solid @borderColor;
      span{
        display: inline-block;
        width: 80px;
        height: 100px;
        border:1px solid @borderColor;
        text-align: center;
        margin: 0 10px;
        font-size: 14px;
        cursor: pointer;
        &:hover{
          color:@thirdColor;
        }
        i{
          display: block;
          font-size: 30px;
          margin-top: 20px;
          margin-bottom: 10px;
        }
      }
    }
    .intro{
      font-size: 18px;
      border-right: 1px solid @borderColor;
      .intro-p{
        font-size: 13px;
        width: 300px;
        margin: 0 auto;
        text-align: left;
        color:@fontDeepColor;
        line-height: 1.8em;
      }
      div{
        text-align: left;
        font-size: 14px;
        margin-left: 47px;
        margin-top: 20px;
        img{
          margin: 0 6px;
          display: inline-block;
          vertical-align: middle;
        }
      }
    }
    .code{
      font-size: 18px;
      img{
        display: block;
        margin: 0 auto;
      }
      span{
        font-size: 12px;
        color:@thirdColor;
        margin-top: 10px;
        display: block;
      }
    }
  }
  footer{

    background-color :@thirdColor;
    width: 100%;
    height: 208px;
    color:white;
    overflow: hidden;
    .footerBottom{
      margin-top: 30px;
      font-size: 13px;
      text-align: center;
      ul{
        li{
          display: inline-block;
          cursor: pointer;
          padding: 0 6px;
          border-right: 2px solid @fontDefaultColor;
          &:last-child{
            border-right:none;
          }
        }
      }
      p{
        margin-top: 5px;
      }
    }
  }

}
</style>
