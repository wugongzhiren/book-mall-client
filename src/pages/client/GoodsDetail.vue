<template>
  <div class="GoodsDetail">

    <div class="content">
      <el-card>
      <div class="goodsInfo">
        <img class="infoLeft" :src="goodsImg" alt="商品图片"/>
        <div class="infoRight">
          <van-cell-group>
            <van-cell title="书名">
              {{goodsName}}
            </van-cell>
            <van-cell title="作者">
              {{author}}
            </van-cell>
            <van-cell title="类型">
              <span v-if="typeId=='1'">国学</span>
              <span v-else-if="typeId=='2'">教辅</span>
              <span v-else-if="typeId=='3'">专业类</span>
              <span v-else-if="typeId=='4'">小说</span>
              <span v-else-if="typeId=='5'">计算机</span>
              <span v-else-if="typeId=='6'">漫画</span>
            </van-cell>
            <van-cell title="单价">
              {{price}}
            </van-cell>
            <van-cell title="库存">
              {{'还剩'+stockNum+'件'}}
            </van-cell>
            <van-cell title="出版社">
              {{publish}}
            </van-cell>
          </van-cell-group>

          <van-cell-group class="cell-detail">
            <van-cell class="good-count" title="数量">
              <van-stepper
                :max=temStockNum
                :min="1"
                input-width="60px"
                integer
                v-model="num"
              />
            </van-cell>
          </van-cell-group>
         <!-- <div class="infoBox">
            <h3 class="name">{{goodsName}}</h3>
          </div>
          <div class="infoBox">
            <p>{{goodsDesc}}</p>
          </div>
          <div class="infoBox">
            <h3 class="price">{{'¥'+price}}</h3>
          </div>
          <div class="infoBox">
            <span>类型：</span>
            &lt;!&ndash; <Radio v-for="(item,index) in specs" :key="item.id" v-model="temSpecId" :initVal="specs[0].id" radioName="spec" :radioVal="item.id">

             </Radio>&ndash;&gt;
            <span v-if="typeId=='1'" class="tips" slot="tips">经典系列</span>
            <span v-else-if="typeId=='2'" class="tips" slot="tips">儿童系列</span>
            <span v-else-if="typeId=='3'" class="tips" slot="tips">奶油系列</span>
            <span v-else-if="typeId=='4'" class="tips" slot="tips">尊爱系列</span>
          </div>
          <div class="infoBox">
            <span class="tips" slot="tips">{{'还剩'+stockNum+'件'}}</span>
          </div>
          <div class="infoBox">
            <span>数量：</span>
            <NumberInput v-model="num" :min="1" :max="temStockNum"/>
          </div>-->

          <van-button type="primary" @click="buy">立即购买</van-button>
          <!--<button class="buyBtn" @click="buy">立即购买</button>-->
          <van-button type="primary" @click="addToCart">加入购物车</van-button>
          <van-button type="primary" @click="readOnline">在线阅读</van-button>
         <!-- <button v-if="isCollect" @click="collect('1')">取消收藏</button>
          <button v-else @click="collect('0')">收藏</button>-->
        </div>
      </div>

        <div style="margin: 20px;line-height: 30px"><span style="font-weight: bold">简介：</span>{{description}}</div>
      </el-card>
      <!--<section class="msgBox leftContainer">


      </section>-->

    </div>

     <!-- <span class="name">{{goodsName}}</span>
      <div class="good">
       &lt;!&ndash; <div >
          <span class="name">sssss{{goodsName}}</span>
          <div class="title">ssss{{goodsName}}</div>
          <div class="description">ssss{{description}}</div>
          <div class="price">
            <span class="sell-price">d11{{price}}</span>
            <span class="sale-price">
            <del>11{{price}}</del>
          </span>
          </div>
        </div>&ndash;&gt;
        <van-cell-group class="cell-detail">
          <van-cell class="good-count" title="数量">
            <van-stepper
              :max="10"
              :min="1"
              input-width="60px"
              integer
              v-model="stock"
            />
          </van-cell>
        </van-cell-group>

        <van-cell-group>
          <van-cell is-link title="作者">
            {{goodsName}}
          </van-cell>
          <van-cell is-link title="出版社">
            {{goodsName}}
          </van-cell>
        </van-cell-group>
        <van-cell-group class="cell-detail">
          <van-cell @click="scrollToDetail"
                    is-link
                    title="查看商品详情"/>
        </van-cell-group>
        <div class="detail"
             ref="detail">
          <img :src="goodsImg">
        </div>
        <van-goods-action>
          <van-goods-action-mini-btn icon="shopping-cart-o"
                                     text="购物车"
                                     to="/cartIndex"/>
          <van-goods-action-mini-btn icon="wap-home"
                                     text="首页"
                                     to="/"/>
          <van-goods-action-big-btn @click="addCartClick"
                                    text="加入购物车"/>
          <van-goods-action-big-btn @click="bugNowClick"
                                    primary
                                    text="立即购买"/>
        </van-goods-action>
      </div>
-->

  </div>
</template>

<script>
  import {mapState} from 'vuex';
  import {
    getGoodsInfo,
    saveCollect,
    getGoodCollects,
    addOrder,
    getTickets,
    deleteTicketByValue,
    getGoodsList,
    deleteTicket, getAllTicket
  } from '../../api/client';
  import NumberInput from '../../components/NumberInput';
  import Radio from '../../components/Radio';
  import GoodsItem from '../../components/GoodsItem';

  export default {
    name: 'GoodsDetail',
    components: {
      NumberInput,
      Radio,
      GoodsItem
    },
    computed: {
      ...mapState([
        'clientToken',
        'clientName'
      ]),
      id() {
        return this.$route.params.id;
      },
      /*goodsPrice(){
        let unitPrice = 0;
        this.specs.map((item,index)=>{
          if(item.id===this.temSpecId){
            unitPrice = Number(item.unitPrice);
          }
        })
        return (this.num*unitPrice);
      },*/
      filterList() {
        return this.goodsList.filter((item) => {
          return String(item.id) !== String(this.id);
        })
      }
    },
    data() {
      return {
        ticket: '0',
        goodsImg: '',
        goodsName: '',
        goodsDesc: '',
        price: '',
        description: '',
        stock: '',
        specs: [],
        typeId: '',
        temSpecId: 0,
        num: 1,
        msgList: [],
        askContent: '',
        tagList: ['商品描述'],
        curIndex: 0,
        rate: '',
        ticketList: [],
        goodsList: [],
        dialogVisible: false,
        isCollect:false,
        author:'',
        publish:'',
        temStockNum:9999,
        ticketID:'',
      }
    },

    methods: {
      changeIndex(i) {
        this.curIndex = i;
      },

      getGoodsInfo(id) {
        const res = getGoodsInfo(id);
        res
          .then((data) => {
            this.goodsImg = data.t.imgurl;
            this.goodsName = data.t.name;
            this.goodsDesc = data.t.description;
            //this.specs = data.specs;
            this.typeId = data.t.type;
            this.stockNum = data.t.stock;
            this.temStockNum=parseInt( this.stockNum);
            this.price = data.t.unitPrice;
            this.author=data.t.author;
            this.publish=data.t.publish;
            this.description = data.t.description;
            //this.getTypeGoodsList(data.typeId);
          })
          .catch((e) => {
            alert(e);
          })
      },

      addToCart() {
        if (!this.clientToken) {
          alert('请先登录！');
          return;
        }
        if(this.stockNum=='0'){
          alert('库存不足，无法加入购物车！');
          return;
        }
        const res1 = addOrder({
          goodid:this.id,
          userid: this.clientToken,
          orderName: this.goodsName,
          orderNum: this.num,
          orderPrice: this.price,
          status:'0'
        });
        res1
          .then(() => {
            alert('加入购物车成功！请前往 个人中心->购物车 结算')
          })
          .catch((e) => {
            alert(e);
          })
      },

      buy() {
        if (!this.clientToken) {
          alert('请先登录！');
          return;
        }
        if(this.stockNum=='0'){
          alert('库存不足，无法购买！');
          return;
        }
        //查询优惠券

            //alert(11)
            const res1 = addOrder({
              goodid:this.id,
              userid: this.clientToken,
              orderName: this.goodsName,
              orderNum: this.num,
              orderPrice: this.price,
              status:'1'
            });
            res1
              .then(() => {
                alert('自动付款成功！请耐心等待包裹派送~')
              })
              .catch((e) => {
                alert(e);
              })

      },
      checkTicket(id){
        this.ticketID=id;
      },
      buywithTicket(){
        //优惠券状态更改
        if(this.ticketID==''){
          const res1 = addOrder({
            goodid:this.id,
            userid: this.clientToken,
            orderName: this.goodsName,
            orderNum: this.num,
            orderPrice: this.price,
            salePrice: this.ticket,
            status:'1'
          });
          res1
            .then(() => {
             // this.ticketID='';
              this.dialogVisible=false;
              alert('自动付款成功！请耐心等待包裹派送~');

            })
            .catch((e) => {
              alert(e);
            })
        }else {
          const res = deleteTicket(this.ticketID);
          res
            .then(() => {
              const res1 = addOrder({
                goodid: this.id,
                userid: this.clientToken,
                orderName: this.goodsName,
                orderNum: this.num,
                orderPrice: this.price,
                salePrice: this.ticket,
                status: '1'
              });
              res1
                .then(() => {
                  this.ticketID = '';
                  this.dialogVisible = false;
                  alert('自动付款成功！请耐心等待包裹派送~');

                })
                .catch((e) => {
                  alert(e);
                })
            })
            .catch((e) => {
              alert(e);
            })
        }
      },
      collect(flag){
        if (!this.clientToken) {
          alert('请先登录！');
          return;
        }
        const res1 = saveCollect({
          goodid:this.id,
          userid: this.clientToken,
          flag:flag
        });
        res1
          .then(() => {
            if(flag=='0'){
              alert('收藏成功！请前往 个人中心->我的收藏 查看')
            }
            this.getCollect();
          })
          .catch((e) => {
            alert(e);
          })
      },
      getCollect(){
        const res1 = getGoodCollects(
          this.clientToken,this.id
        );
        res1
          .then((data) => {
            if(data.t!=null){
              this.isCollect=true;
            }else{
              this.isCollect=false;
            }
          })
          .catch((e) => {
            alert(e);
          })
      }
    },
      mounted() {
        this.getGoodsInfo(this.id);

        this.getCollect();
        //this.getComment(this.id);
      },
    }
</script>

<style scoped lang="less">
  @import "../../assets/css/var.less";

  .GoodsDetail {
    .content {
      width: 100%;
      padding-top: 20px;

      .goodsInfo {
        width: 100%;
        overflow: hidden;

        .infoLeft {
          display: inline-block;
          width: 200px;
          height: 200px;
          float: left;
        }

        .infoRight {
          display: inline-block;
          float: right;
          width: 700px;

          .infoBox {
            margin-bottom: 30px;

            .name {
              font-size: 20px;
            }

            p {
              color: @fontDefaultColor;
              font-size: 15px;
            }

            .price {
              font-size: 35px;
              color: @falseColor;
            }

            span {
              color: @fontDefaultColor;
              font-size: 13px;
              display: inline-block;
              width: 50px;
            }

            .tips {
              width: auto;
              margin: 0 20px 0 5px;
            }

            .NumberInput {
              display: inline-block;
              vertical-align: middle;
            }
          }

          button {
            background-color: @falseColor;
            border: none;
            margin: 10px;
           /* width: 130px;
            height: 50px;
            color: white;
            border: none;
            font-size: 14px;
            margin-right: 20px;
            margin-top: 10px;*/

            &:hover {
              opacity: 0.8;
            }
          }

          /*.buyBtn {
            border: 1px solid #b4a078;
            color: #b4a078;
            background-color: #f5f3ef;
          }*/
        }
      }

      .msgBox {
        margin: 50px 0;
        padding-top: 0;

        .tagList {
          width: 100%;
          height: 40px;
          border-bottom: 1px solid @borderColor;
          background-color: #f5f5f5;

          li {
            width: 170px;
            height: 42px;
            position: relative;
            top: -2px;
            display: inline-block;
            text-align: center;
            line-height: 40px;
            font-size: 13px;
            cursor: pointer;

            &:hover {
              color: @thirdColor;
            }
          }

          .selected {
            background-color: white;
            border-top: 4px solid @thirdColor;
          }
        }

        .commentBody {
          padding: 20px;
          min-height: 300px;

          .rateBox {
            margin-bottom: 10px;

            span {
              color: @fontDefaultColor;
              display: inline-block;
              margin-right: 10px;
            }

            .rate {
              color: @falseColor;
              font-weight: 600;
              font-size: 30px;
            }
          }

          .commentList {
            width: 100%;

            li {
              width: 100%;
              display: block;
              margin: 0 auto;
              border-bottom: 1px solid @borderColor;
              padding: 20px 0;

              .userInfo {
                width: 80px;
                display: inline-block;
                text-align: center;

                img {
                  margin: auto;
                  display: block;
                  width: 50px;
                  height: 50px;
                  border-radius: 50%;
                  margin-bottom: 6px;
                }

                span {
                  font-size: 13px;
                  color: @fontDefaultColor;
                }
              }

              .commentInfo {
                display: inline-block;
                vertical-align: top;

                .starList {
                  i {
                    color: #f9bd4f;
                  }
                }

                .specName, .time {
                  color: @fontDefaultColor;
                  font-size: 13px;
                  margin-top: 10px;
                }

                .comment {
                  margin-top: 10px;
                }
              }
            }
          }

          .noComment {
            width: 100%;
            text-align: center;
            color: @thirdColor;
            padding-top: 30px;
          }
        }

        .msgBody {
          padding: 20px;
          min-height: 300px;

          .inputBox {
            margin-top: 20px;

            textarea {
              width: 88%;
              height: 50px;
              padding: 5px;
              border: 2px solid @borderColor;
              display: inline-block;
              overflow: hidden;
            }

            button, .banAsk {
              float: right;
              width: 10%;
              height: 50px;
              position: relative;
              display: inline-block;
              overflow: hidden;
              background-color: white;
              border: 2px solid @fontShallowColor;
              color: @fontDefaultColor;
            }

            .banAsk {
              background-color: @fontShallowColor;
              text-align: center;
              font-size: 12px;
              line-height: 50px;
              color: white;
              user-select: none;
            }
          }

          .msgList {
            margin-top: 20px;

            li {
              border-bottom: 1px solid @borderColor;
              padding: 10px 0;

              .ask, .answer {
                margin: 8px 0;
                width: 100%;

                .note {
                  display: inline-block;
                  color: white;
                  text-align: center;
                  width: 20px;
                  height: 20px;
                  border-radius: 50%;
                  font-size: 10px;
                  line-height: 20px;
                }

                .text {
                  font-size: 14px;
                }

                .tipsInfo {
                  float: right;
                  font-size: 14px;
                  color: @fontDefaultColor;
                }
              }

              .ask {
                .note {
                  background-color: @falseColor;
                }
              }

              .answer {
                .note {
                  background-color: @mainColor;
                }
              }
            }
          }
        }
      }

      .typeGoods {
        margin: 50px 0;
        border: 1px solid @borderColor;
        padding-top: 0;

        .title {
          text-align: center;
          width: 100%;
          height: 40px;
          line-height: 40px;
          background-color: #f5f5f5;
          font-weight: 600;
          border-bottom: 1px solid @borderColor;
        }

        .list {
          .GoodsItem {
            display: block;
            border-bottom: 1px dotted @borderColor;
            margin: 0 auto;
          }
        }
      }
    }

    .ban {
      user-select: none;
      cursor: not-allowed;
    }
  }

 /* .good {
    padding-bottom :50px;
    background-color: #eee;

    .back-btn {
      position :fixed;
      z-index: 999;
      top :10px;
      left :10px;
      width :30px;
      height :30px;
      border-radius :50%;
      color: #fff;
      line-height :32px;
      text-align: center;
      background-color :rgba(102, 102, 102, 0.4);

      i {
        font-weight :700;
          margin-left :-2px;
        margin-top :4px;
      }
    }
  }



  .info {
    padding:4px 10px;
    background-color: #eee;
    height: 300px;
    .name {
      font-size :18px;
      font-weight:bold;
      line-height:28px;
      margin-top:6px;
    }

    .title {
      font-size:14px;
      color: #4d525d;
      line-height :26px;
      word-break: break-all;
    }

    .description {
      font-size :12px;
      color :#969696;
      line-height :18px;
      word-break: break-all;
    }

    .price {
      margin-top: 10px;

      .sell-price {
        color :#f00;
        font-size :18px;
        margin-right: 10px;
      }

      .sale-price {
        color :#969696;
        font-size :12px;
      }
    }
  }

  .van-cell-group {
    border-top: 1px solid #eee;
  }

  .cell-detail {
    margin-top :6px;
    margin-bottom: 6px;
  }



  .detail img {
    width:100%;
    height:auto;
  }*/
</style>
