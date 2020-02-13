<template>
  <div>
    <el-button>数据库数据备份</el-button>
    <el-button>数据库数据恢复</el-button>
  </div>
</template>

<script>
import {getSuggests} from '../../api/client';
export default {
  name: 'EditUser',
  computed:{
  },
  data(){
  	return{
  		suggests:[]
  	}
  },
  mounted(){
  	const res = getSuggests();
  	res
  	.then((data)=>{
  		this.suggests = data.t;
  	})
  	.catch((e)=>{
  		alert(e)
  	})
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
