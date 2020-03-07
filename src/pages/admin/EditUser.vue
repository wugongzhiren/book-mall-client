<template>
  <div class="EditUser">
  	<header class="clear">
  		<span>用户管理</span>
    </header>
      <div class="content">
        <el-table
          :data="userList"
          style="width: 100%">
          <el-table-column
            prop="id"
            label="ID"
            width="180">
          </el-table-column>
          <el-table-column
            prop="userid"
            label="用户ID"
            width="180">
          </el-table-column>
          <el-table-column
            prop="username"
            label="用户名">
          </el-table-column>
          <el-table-column
            prop="phone"
            label="联系方式">
          </el-table-column>
          <el-table-column
            prop="address"
            label="用户地址">
          </el-table-column>
        </el-table>
      </div>

  </div>
</template>

<script>
import {getAllUser,getSearchUser,deleteUser} from '../../api/admin';
export default {
  name: 'EditUser',
  computed:{
  },
  data(){
  	return{
  		userList:[]
  	}
  },
  mounted(){
  	const res = getAllUser();
  	res
  	.then((users)=>{
  		this.userList = users.t;
  	})
  	.catch((e)=>{
  		alert(e)
  	})
  },
  methods:{
  	deleteUser(id){
  		const res = deleteUser(id);
  		res
  		.then(()=>{
  			alert('删除成功');
  			this.userList.map((item,index)=>{
  				if(item.id===id){
  					this.userList.splice(index,1);
  				}
  			})
  		})
  		.catch((e)=>{
  			alert(e);
  		})
  	},
  	searchUser(){
  		const val = this.$refs.input.value;
  		const res = getSearchUser(val);
  		res
  		.then((data)=>{
  			this.userList = data;
  		})
  		.catch((e)=>{
  			alert(e);
  		})
  	},
  }
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
      font-size: 18px;
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
