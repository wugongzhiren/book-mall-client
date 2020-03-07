<template>
  <div class="Goods">
    <header class="clear">
      <span>商品管理</span>
      <el-button type="danger" @click="centerDialogVisible = true">添加入库</el-button>
    </header>
    <!--<Tag :tagList="tagTextList" @indexChange="changeTag"/>-->
    <div class="content">
      <el-table
        :data="bookList"
        style="width: 100%"
        :row-class-name="tableRowClassName">
        <el-table-column
          prop="id"
          label="ID"
          width="180">
        </el-table-column>
        <el-table-column
          prop="name"
          label="书名"
          width="180">
        </el-table-column>
        <el-table-column
          prop="author"
          label="作者">
        </el-table-column>
        <el-table-column
          prop="publish"
          label="出版社">
        </el-table-column>
        <el-table-column
          prop="unitPrice"
          label="单价">
        </el-table-column>
        <el-table-column
        prop="stock"
        label="库存">
      </el-table-column>
        <el-table-column
          prop="offlineUrl"
          label="离线地址">
        </el-table-column>
        <el-table-column
          label="商品预览">
          <template slot-scope="scope">
            <img style="width: 120px;height: 120px" :src="scope.row.imgurl" />
          </template>
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>

      <el-dialog
        title="添加商品"
        :visible.sync="centerDialogVisible"
        width="40%"
        center>
        <el-row>书名：<el-input style="width: 480px" v-model="name" placeholder="请输入书名"></el-input></el-row>
        <br>
        <el-row>作者：<el-input style="width: 480px" v-model="author" placeholder="请输入作者"></el-input></el-row>
        <br>
        <el-row>出版社：<el-input style="width: 480px" v-model="publish" placeholder="请输入出版社"></el-input></el-row>
        <br>
        <el-row>类型：<el-radio v-model="type" label="1">国学</el-radio>
          <el-radio v-model="type" label="2">教辅</el-radio>
          <el-radio v-model="type" label="3">专业类</el-radio>
          <el-radio v-model="type" label="4">小说</el-radio>
          <el-radio v-model="type" label="5">计算机</el-radio>
          <el-radio v-model="type" label="6">漫画</el-radio></el-row>
        <br>
        <el-row>单价：<el-input style="width: 480px" v-model="unitPrice" placeholder="请输入单价"></el-input></el-row>
        <br>
        <el-row>库存：<el-input style="width: 480px" v-model="stock" placeholder="请输入库存"></el-input></el-row>
        <br>
        <el-row>URL：<el-input style="width: 480px" v-model="imgUrl" placeholder="请输入图片URL"></el-input></el-row>
        <br>
        <el-row>在线阅读地址：<el-input style="width: 480px" v-model="offlineUrl" placeholder="请输入在线阅读地址"></el-input></el-row>
        <br>
        <el-row>简介：<el-input style="width: 480px" type="textarea" :rows="4" v-model="description" placeholder="请输入简介内容"></el-input></el-row>

        <span slot="footer" class="dialog-footer">
    <el-button @click="centerDialogVisible = false">取 消</el-button>
    <el-button type="danger" @click="saveChange">确 定</el-button>
  </span>
      </el-dialog>
      <!--<ul class="clear">
        <li v-for="(item,index) in bookList" :key="'goods'+item.id">
          <img :src="item.imgurl" alt="" />
          <span>{{item.goodsname}}</span>
          <div>
            <button class="normalBtn" @click="navTo('/backstage/goods/'+item.id)">编辑</button>
            <button @click="deleteGoods(item.id)" class="deleteBtn">删除</button>
          </div>
        </li>
        <li>
          <div class="addGoods" @click="navTo('/backstage/goods/new')">
            <div>+</div>
            点击增加商品
          </div>
        </li>
      </ul>-->
    </div>
  </div>
</template>

<script>
  import {getGoods, getTypes, addType, deleteGoods, addGoods, updateGoods} from '../../api/admin';
  import Tag from '../../components/Tag';

  export default {
    name: 'Goods',
    components: {
      Tag,
    },
    computed: {
      tagTextList() {
        let temArr = [];
        this.tags.map((item, index) => {
          temArr.push(item.name)
        });
        return temArr;
      }
    },
    data() {
      return {
        tags: [],
        centerDialogVisible:false,
        bookList: [],
        popupShow: false,
        id:'',
        type:'1',
        name: '',
        imgUrl: '',
        description: '',
        stock: 0,
        unitPrice: 0,
        author:'',
        publish:'',
        offlineUrl:'',
        flag:'A',
      }
    },
    methods: {
      tableRowClassName({row, rowIndex}) {
        if (rowIndex === 1) {
          return 'warning-row';
        } else if (rowIndex === 3) {
          return 'success-row';
        }
        return '';
      },
      getGoods() {
        const res = getGoods();
        res
          .then((goods) => {
            this.bookList = goods.t;
          })
          .catch((e) => {
            alert(e);
          })
      },
      navTo(route) {
        this.$router.push(route);
      },
      deleteGoods(id) {
        const res = deleteGoods(id);
        res
          .then(() => {
            this.bookList.map((item, index) => {
              if (item.id === id) {
                this.bookList.splice(index, 1)
              }
            })
          })
          .catch((e) => {
            alert(e);
          })
      },
      handleEdit(index, row) {
        this.id=row.id;
        this.type = row.type;
        this.name = row.name;
        this.imgUrl = row.imgurl;
        this.description = row.description;
        this.stock = row.stock;
        this.unitPrice=row.unitPrice;
        this.author=row.author;
        this.publish=row.publish;
        this.offlineUrl=row.offlineUrl;
        this.flag='U';
        this.centerDialogVisible=true;
      },
      handleDelete(index, row) {
        deleteGoods(row.id);
      },
      saveChange() {
        if(this.name==''||this.type==''||this.imgUrl==''||this.description==''||this.stock==''||this.unitPrice==''||this.author==''||this.publish==''){
          this.$message('请输入完整信息');
          return;
        }
        this.centerDialogVisible=false;

          const res = addGoods({
            id:this.id,
            name: this.name,
            type: this.type,
            imgUrl: this.imgUrl,
            description: this.description,
            stock: this.stock,
            unitPrice: this.unitPrice,
            author:this.author,
            publish:this.publish,
            flag:this.flag,
            offlineUrl:this.offlineUrl
          });
          res
            .then((data) => {
              this.id='';
              this.type = '';
              this.name = '';
              this.imgUrl = '';
              this.description ='';
              this.stock =0;
              this.unitPrice=0;
              this.author='';
              this.publish='';
              this.offlineUrl='';
              this.flag='A';
              if (data.code == 200) {
                alert("操作成功!");
              } else {
                alert("该商品已经被添加过!");
              }
            })
            .catch((e) => {
              alert(e);
            })
      },
    },
    mounted() {
      this.getGoods();
    }
  }
</script>

<style scoped lang="less">
  @import "../../assets/css/var.less";

  .Goods {
    header {
      width: 100%;
      height: 40px;
      line-height: 40px;

      span {
        float: left;
        font-size: 18px;
      }

      button {
        float: right;
      }
    }

    .content {
      position: relative;
      width: 100%;
      top: 10px;

      ul {
        padding: 10px;

        li {
          float: left;
          display: inline-block;
          width: 200px;
          height: 300px;
          text-align: center;
          margin-right: 20px;

          img {
            width: 100%;
            height: 200px;
            border: 1px solid @borderColor;
          }

          span {
            font-size: 13px;
            display: block;
            margin: 10px 0;
            color: @fontDefaultColor;
          }

          .normalBtn {
            width: 50px;
            height: 25px;
            color: @mainColor;
            border: 1px solid @mainColor;
            background-color: white;
            border-radius: 5px;
            margin-right: 5px;
          }

          .deleteBtn {
            width: 50px;
            height: 25px;
            color: @falseColor;
            border: 1px solid @falseColor;
            background-color: white;
            border-radius: 5px;
          }

          .addGoods {
            width: 100%;
            height: 200px;
            text-align: center;
            cursor: pointer;
            border: 1px solid @borderColor;
            color: @fontDefaultColor;

            div {
              margin: 50px auto 10px;
              border-radius: 50%;
              border: 2px solid @fontDefaultColor;
              width: 40px;
              height: 40px;
              font-size: 30px;
              text-align: center;
              line-height: 30px;
            }
          }

          &:last-of-type {
            margin-right: 0;
          }
        }
      }
    }

    .popupContent {
      padding: 20px;

      input {
        display: block;
        width: 200px;
        height: 30px;
        border: none;
        border-bottom: 2px solid #333333;
        margin-top: 20px;
      }

      button {
        display: block;
        margin: 30px auto 0;
        background-color: #333333;
        color: white;
        border: none;
        width: 80px;
        height: 30px;
        border-radius: 5px;
      }
    }
  }
</style>
