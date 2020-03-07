<template>
  <div class="EditGoods">
    <header class="clear">
      <span>公告设置</span>
    </header>
    <div class="content">
      <div class="inputBox">
        <span>文字公告：</span>
        <TextInput class="long" placeholder="请输入公告1" v-model="tips" />
      </div>
      <div class="inputBox">
        <span>图片公告：</span>
        <TextInput class="long" placeholder="请输入图片地址" v-model="img" />
      </div>
      <el-button type="danger" @click="saveChange">保存</el-button>
    </div>
  </div>
</template>

<script>
  import TextInput from '../../components/TextInput';
  import {getAds,addAds} from '../../api/client';
  import {addGoods} from "../../api/admin";

  export default {
    name: 'EditAds',
    components: {
      TextInput
    },
    data() {
      return {
        id:'',
        tips: '',
        img: '',
      }
    },
    methods: {
      back() {
        this.$router.go(-1);
      },
      saveChange(){
        if(this.tips==''||this.img==''){
          this.$message("请输入完整的信息!");
          return;
        }
        const res = addAds({
          id: this.id,
          tips: this.tips,
          img:this.img
        });
        res
          .then((data) => {
            if (data.code == 200) {
              alert("保存成功!");
            }
          })
          .catch((e) => {
            alert(e);
          })
      }
    },
    mounted() {
      //新建商品

      const res = getAds();
      res
        .then((data) => {
          if(data.t.length>0) {
            this.id = data.t[0].id;
            this.tips = data.t[0].tips;
            this.img = data.t[0].img;
          }
        })
        .catch((e) => {
          alert(e);
        })
    }

  }
</script>

<style scoped lang="less">
  @import "../../assets/css/var.less";

  .EditGoods {
    header {
      width: 100%;
      height: 40px;
      line-height: 40px;

      span {
        float: left;
        font-size: 18px;
      }
    }

    .content {
      width: 100%;
      background-color: white;
      padding: 10px;
      font-size: 18px;
      textarea {
        width: 80%;
        height: 300px;
        border: 1px solid @borderColor;
      }

      .inputBox {
        margin-bottom: 30px;

        span {
          width: 90px;
          display: inline-block;
          color: @fontDefaultColor;
          font-weight: 600;
          vertical-align: middle;
        }

        .verTop {
          vertical-align: top;
        }

        .tips {
          font-weight: normal;
          width: auto;
          font-size: 13px;
          position: relative;
          left: 3px;
        }

        .val {
          width: auto;
          font-weight: 500;
          color: @fontDeepColor;
        }

        ul {
          display: inline-block;
          width: 500px;

          li {
            width: 100%;
            height: 40px;

            span {
              width: auto;
              font-size: 13px;
              font-weight: 500;
            }

            input {
              margin-right: 10px;
            }

            button {
              width: 50px;
              height: 25px;
              color: #d7514a;
              border: none;
              background-color: white;
              border-radius: 5px;
            }
          }

          .addSpec {
            text-align: center;
            line-height: 30px;
            background-color: #409EFF;
            color: white;
            border: none;
            width: 100px;
            height: 30px;
            border-radius: 5px;
            font-size: 13px;
            cursor: pointer;
            margin: 10px auto;
          }
        }

        .long {
          width: 900px;
        }

        .numInput {
          width: 30px;
          text-align: center;
          border: none;
          border-bottom: 2px solid @mainColor;
        }
      }

      .btnBox {
        margin: auto;
        display: block;
        width: 250px;
      }

      .confirmBtn {
        display: inline-block;
        margin-right: 30px;
        background-color: #409EFF;
        color: white;
        border: none;
        width: 100px;
        height: 30px;
        border-radius: 5px;
        cursor: pointer;
      }

      .normalBtn {
        display: inline-block;
        background-color: grey;
        color: white;
        border: none;
        width: 100px;
        height: 30px;
        border-radius: 5px;
        cursor: pointer;
      }
    }

    .popupContent {
      padding: 10px;

      input {
        display: block;
        width: 400px;
        margin-bottom: 10px;
        height: 30px;
      }

      button {
        display: block;
        margin: auto;
        background-color: #333333;
        color: white;
        border: none;
        width: 100px;
        height: 30px;
        border-radius: 5px;
        cursor: pointer;
      }
    }
  }
</style>
