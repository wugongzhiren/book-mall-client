<template>
  <div class="MallIndex">
    <div class="home-swipe">
      <div class="home-swipe-head">
        <span class="recommend">今日推荐</span>
        <span class="tips">每天都有新发现</span>
        <span class="swipe-num">
    </span>
      </div>
      <FadeSwiper class="swiperBox" height="400px">
        <img class="banner" slot="item1" src="/src/assets/img/bannel1.jpg" />
        <img class="banner" slot="item2" src="/src/assets/img/bannel2.jpg" />
        <img class="banner" slot="item3" src="/src/assets/img/bannel3.jpg" />
      </FadeSwiper>
    </div>
<div>
  <good-item describe="每日热销指南"
             title="热销榜" moreLink="更多">
    <ul class="hot-ul">
      <li :key="item.id"
          @click="showGood(item.id)"
          class="sale-item"
          v-for="item in bookList.slice(0,4)">
        <el-card style="margin: 10px">
        <img :src="item.imgurl"
             alt="">
        <div class="sale-name">{{item.name}}</div>
        <div class="sale-price">￥{{item.unitPrice}}</div>
        </el-card>
      </li>
    </ul>
  </good-item>
 <good-item describe="前所未有的折扣"
             title="低价促销">
    <ul class="sale-ul">
      <li :key="item.id"
          @click="showGood(item.id)"
          class="li-item"
          v-for="(item) in bookList.slice(0,4)">
        <el-card style="margin: 10px">
        <div class="img">
          <img :src="item.imgurl">
        </div>
        <div class="title">
          {{item.name}}
        </div>
        <div class="price">
            <span class="new-price">
              ￥{{item.unitPrice}}
            </span>
        </div>
        </el-card>
      </li>
    </ul>
  </good-item>
  <good-item describe="发现更多优质好货"
             title="发现">
    <ul class="discover-ul">
     <backgroundImg :class="index===0?'discover-img':''"
                      :desc="item.description"
                      :imgSrc="item.imgurl"
                      :key="item.id"
                      :topic="item.name"
                      @click.native="showGood(item.id)"
                      class="discover-li"
                      v-for="(item,index) in bookList.slice(0,2)"/>
    </ul>
    <ul class="discover-ul">
      <backgroundImg :desc="item.description"
                      :imgSrc="item.imgurl"
                      :key="item.id"
                      :topic="item.name"
                      @click.native="showGood(item.id)"
                      class="discover-li"
                      v-for="item in bookList.slice(2,5)"/>
    </ul>
  </good-item>
</div>

  </div>
</template>

<script>
import {getGoods,getTypes,addType,deleteGoods} from '../../api/admin';
import SectionHeader from '../../components/SectionHeader';
import ZoomImg from '../../components/ZoomImg';
import GoodsItem from '../../components/GoodsItem';
import goodItem from "../../components/goodItem";
import backgroundImg from "../../components/backgroundImg";
import TypeItem from '../../components/TypeItem'
import Slick from '../../components/Slick';
import FadeSwiper from '../../components/FadeSwiper';

import {getClientSize,getScrollWidth} from '../../util/util';
import {getAds} from "../../api/client";

export default {
  name: 'MallIndex',
  components:{
    backgroundImg,
    SectionHeader,
    ZoomImg,
    GoodsItem,
    TypeItem,
    Slick,
    FadeSwiper,
    goodItem
  },
  computed:{
    clientWidth(){
      return getClientSize().width-getScrollWidth()+'px';
    }
  },
  data () {
    return {
      bookList:[],
      initTimestamp:0,
      newTimestamp:0,
      timer:null,
      h:0,
      m:0,
      s:0,
    }
  },

  methods:{
    filterGoodsByType(typeid){
      return this.goodsList.filter((item)=>{
        return item.typeId===typeid;
      });
    },
    navTo(route){
      this.$router.push(route);
    },
    selectType(typeId){
      if(typeId==-1){
        return;
      }
      this.navTo('/mall/show/goodsList/'+typeId+'/all');
    },
    getGoodsList(typeId){
      if(typeId===0){
        //获取所有
        const res = getGoods();
        res
          .then((goods)=>{
            this.bookList = goods.t;
          })
          .catch((e)=>{
            alert(e);
          })
      }else{
        //根据类型获取

      }
    },
    playVideo(){
      var vdo = document.getElementById("videoPlay");
      vdo.play();
    },

    searchTip(tip){
      alert(tip)
    },
    showGood(id){
      this.$router.push('/mall/goods/'+id);
    },
    inputTextChange(text){
    },
    scrollHandle(){
      if(top>0){
        this.navShouldFixed=false;
      }
      //已经到顶
      else{
        this.navShouldFixed=true;
      }
    }
  },
  created() {
    const res = getAds();
    res
      .then((data) => {
        if(data.t.length>0) {
          this.img1=(data.t[0].img1);
          this.img2=(data.t[0].img2);
          this.img3=(data.t[0].img3);
          //alert( data.t[0].tips1)
           this.tip1 = data.t.tip1;
           this.tip2 = data.t.tip2;
           this.tip3 = data.t.tip3;
           this.img1 = data.t.img1;
           this.img2 = data.t.img2;
           this.img3 = data.t.img3;
        }
      })
      .catch((e) => {
        alert(e);
      })
  },
  mounted(){
    //获取数据
    this.getGoodsList(0);
    //记录打开网页再加四小时的时间
    this.initTimestamp = new Date().getTime()+(4*60*60*1000);
    this.timer = setInterval(()=>{
      this.newTimestamp = new Date().getTime();
      let diff = parseInt((this.initTimestamp-this.newTimestamp)/1000);
      diff = diff%(86400*365)%(86400*30)%86400;
      this.h = new String(Math.floor(diff/3600)).padStart(2,'0');
      diff = diff%3600;
      this.m = new String(Math.floor(diff/60)).padStart(2,'0');
      diff = diff%60;
      this.s = new String(diff).padStart(2,'0');
    },1000);
  },

  beforeDestroy(){
    clearInterval(this.timer);
    this.timer = null;
  },

}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
.MallIndex{
  /*width: 100%;*/
  height: 1600px;
  .section{
    padding:30px;
    overflow: hidden;
    .Slick{
      position: relative;
      left: -30px;
    }
  }
  .newGoods{
    margin-top: 440px;
  }
  .flashSale{
    .content{
      font-size: 18px;
      border: 1px solid @borderColor;
      height: 676px;
      position: relative;
      .left{
        vertical-align: top;
        display: inline-block;
        width: 20%;
        height: 100%;
        position: relative;
        background-image: url(//yanxuan.nosdn.127.net/c9aeb62a3f79123d793d8c49b6698b09.jpg);
        background-repeat: no-repeat;
        background-size: 100%;
        text-align: center;
        color: #615548;
        cursor: pointer;
        .title{
          font-size: 26px;
          font-weight: 500;
          margin-top: 60px;
        }
        hr{
          width: 20px;
          height: 2px;
          background-color: #615548;
          border: none;
          margin-top: 20px;
        }
        .tips{
          font-size: 18px;
          margin-top: 20px;
        }
        .countBox{
          margin-top: 20px;
          .time{
            width: 42px;
            height: 42px;
            line-height: 42px;
            font-size: 19px;
            display: inline-block;
            color: white;
            background-color: #615548;
          }
        }
        .allBtn{
          width:110px;
          height: 33px;
          line-height: 33px;
          font-size: 13px;
          background-color: #a7936e;
          color:white;
          border-radius: 20px;
          margin:50px auto;
        }
      }
      .right{
        vertical-align: top;
        display: inline-block;
        width: 80%;
        height: 100%;
        position: relative;
        overflow: hidden;
        li{
          display: inline-block;
          width: 50%;
          height: 50%;
          overflow: hidden;
          .leftImg{
            width: 180px;
            height: 100%;
            display: inline-block;
          }
          .rightBox{
            display: inline-block;
            width: 240px;
            height: 100%;
            overflow: hidden;
            .goodsName{
              font-size: 15px;
              cursor: pointer;
              margin-top: 16px;
              &:hover{
                color:@thirdColor;
              }
            }
            .less{
              margin-top: 16px;
              .lessBar{
                display: inline-block;
                width: 150px;
                height: 10px;
                background-color: #ffe5e5;
                border:1px solid #f2cecd;
                border-radius: 10px;
              }
              .lessNum{
                color:@fontDefaultColor;
                font-size: 13px;
              }
            }
            .price{
              margin-top: 16px;
              .nowPrice{
                color: @falseColor;
                font-size: 20px;
              }
              .beforePrice{
                color:@fontDefaultColor;
                text-decoration:line-through;
                margin-left: 10px;
                font-size: 14px;
              }
            }
            .buyBtn{
              margin-top: 16px;
              width: 120px;
              height: 30px;
              color:white;
              background-color: @falseColor;
              text-align: center;
              line-height: 30px;
              cursor: pointer;
            }
          }
          &:nth-of-type(1){
            border-bottom: 1px solid @borderColor;
            border-right: 1px solid @borderColor;
          }
          &:nth-of-type(2){
            border-bottom: 1px solid @borderColor;
          }
          &:nth-of-type(3){
            border-right: 1px solid @borderColor;
          }
        }
      }
    }
  }
  .maker{
    .content{
      .left,.center,.right{
        display: inline-block;
        height: 320px;
        width: 32%;
      }
      .left,.center{
        margin-right: 7px;
        .makerInfo{
          p{
            text-align: center;
          }
          hr{
            width: 60px;
          }
          .large{
            margin-top: 50px;
            margin-bottom: 10px;
            font-size: 26px;
          }
          .small{
            margin-top: 10px;
            font-size: 14px;
          }
        }
      }
      .right{
        .ZoomImg{
          width: 100%;
          height: 49%;
          .makerInfo{
            p{
              text-align: left;
              margin-left: 30px;
            }
            hr{
              width: 60px;
              position: relative;
              left: -122px;
            }
            .large{
              margin-top: 30px;
              margin-bottom: 10px;
              font-size: 20px;
            }
            .small{
              margin-top: 10px;
              font-size: 12px;
            }
          }
          &:first-child{
            margin-bottom:7px;
          }
        }
      }
    }
  }
  .hotGoods{

    height: 624px;
    .left{
      height: 100%;
      width: 394px;
      display: inline-block;
      vertical-align: top;
      .GoodsItem{
        width: 100%;
        height: 520px;
        background-color: white;
      }
      .GoodsItem /deep/ .imgBox{
        height: 380px;
      }
      .GoodsItem /deep/ .goodsInfo{
        height: 130px;
      }
      .GoodsItem /deep/ .goodsName{
        font-size: 30px;
        line-height: 60px;
      }
    }
    .right{
      display: inline-block;
      vertical-align: top;
      width: 740px;
      height: 100%;
      .GoodsItem{
        width: 230px;
        height: 260px;
        background-color: white;
        margin-left: 10px;
        overflow: hidden;
      }
      .GoodsItem /deep/ .imgBox{
        height: 180px;
      }
    }
  }
}
.home-swipe {
  box-sizing :border-box;
  padding: 4px 6px;
  background-color: #fff;
  border-bottom: 1px solid #dedede;

  .home-swipe-head {
    padding: 4px 0;

    .recommend {
      font-size: 20px;
      font-weight: 700
    }

    .tips {
      font-size: 14px;
      margin-left: 6px;
      color: #8f8f8f;
    }

    .swipe-num {
      float: right;

      .indexPage {
        font-weight :700
      }

      .pageNum {
        font-size: 12px;
        color: #8f8f8f;
      }
    }
  }
}
.hot-ul {
  display:flex;

  .sale-item {
    flex-grow: 1;
    width :25%;
    overflow: hidden;
    padding :2px 4px;
    text-align :center;

    img {
      height: 200px;
    }

    .sale-name {
      overflow: hidden;
      text-overflow: ellipsis;
      white-space :nowrap;
      margin: 10px;
    }

    .sale-price {
      color: #ff4c0a;
      font-size:16px;
    }
  }
}
.sale-ul {
  display: flex;
  margin-bottom: 4px;

  .li-item {
    flex-grow: 1;
    width :25%;
    overflow: hidden;
    padding: 2px 4px;
    text-align :center;
    img {
      height: 200px;
    }

    .title {
      margin: 10px;
      overflow: hidden;
      text-overflow :ellipsis;
      white-space: nowrap;
    }

    .new-price {
      margin: 10px;
      color :#ff4c0a;
    }

    .old-price {
      font-size :12px;
      color: #333;
      margin-left :6px;
    }
  }
}

.discover-ul {
  display :flex;
  height :200px;
  margin-bottom: 4px;

  .discover-img {
    width :80px;
  }

  .discover-li {
    flex-grow: 1;
    margin-right: 4px;

    &:last-child {
      margin-right: 0;
    }
  }
}
</style>
