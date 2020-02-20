<template>
  <div>
    <el-button @click="backUp">数据库数据备份</el-button>
    <el-button @click="show = true">数据库数据恢复</el-button>
    <p style="margin: 20px;font-family: 黑体;color: red;font-size: larger">{{msg}}</p>
    <el-dialog
      title="提示"
      :visible.sync="show"
      width="30%"
      center>
    <div> <span>请确认是否恢复数据？</span></div>
      <span slot="footer" class="dialog-footer">
    <el-button @click="show = false">取 消</el-button>
    <el-button type="primary" @click="restore">确 定</el-button>
      </span>
    </el-dialog>
  </div>

</template>

<script>
  import {backUpDb, restoreDb} from '../../api/client';
export default {
  name: 'EditUser',
  computed:{
  },
  data(){
  	return{
  	  msg:'',
      show:false,
  	}
  },
  methods:{
    backUp(){
      const res = backUpDb();
      res
        .then((data) => {
          this.msg=data.msg;

        })
        .catch((e) => {
          alert(e);
        })
    },
    restore(){
      const res = restoreDb();
      res
        .then((data) => {
          alert("恢复成功")

        })
        .catch((e) => {
          alert(e);
        })
    }
  },


}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
.EditUser{
	header{
		width: 100%;
		height: 40px;
		line-height: 40px;
		span{
			float: left;
		}
		div{
			height: 20px;
			float: right;
      input{
        border: none;
        border-bottom: 1px solid #337da4;
        background-color: rgba(0,0,0,0);
        width: 180px;
        padding-left: 10px;
      }
			button{
        position: relative;
        top: -1px;
        border: none;
        background-color: rgba(0,0,0,0);
				i{
					font-size: 17px;
          color:#337da4;
				}
			}
		}
	}
}
</style>
