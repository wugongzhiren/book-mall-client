<template>
  <div class="MallShow">
    <router-view></router-view>
  </div>
</template>

<script>
import TipsInput from '../../components/TipsInput';
import FixedNav from '../../components/FixedNav';

export default {
  name: 'MallShow',
  components:{
    TipsInput,
    FixedNav,
  },
  computed:{
    curPath(){
      return this.$route.path;
    }
  },
  data () {
    return {
      tips:[],
      searchText:'',
      navShouldFixed:false,
    }
  },

  methods:{
    navTo(route){
      this.$router.push(route);
    },
    judgeCurPath(typeId){
      if(typeId===-1){
        if(this.curPath.indexOf('/show/index')>-1){
          return true;
        }else{
          return false;
        }
      }else{
        if(this.curPath.indexOf(`/show/goodsList/${typeId}`)>-1){
          return true;
        }else{
          return false;
        }
      }
    },
    selectType(typeId){
      //首页
      if(typeId===-1){
        this.navTo('/mall/show/index');
      }else{
        this.navTo('/mall/show/goodsList/'+typeId+'/all');
      }
    },
    searchTip(tip){
      alert(tip)
    },
    searchTextChange(text){
    },

    scrollHandle(){

    }
  },

  mounted(){
    //监听滚动事件
    document.addEventListener('scroll',this.scrollHandle,false);
  },

  destroyed(){
    document.removeEventListener('scroll',this.scrollHandle,false);
  },
  watch:{
    searchText(newVal,oldVal){
      this.searchTextChange(newVal);
    }
  }
}
</script>

<style scoped lang="less">
@import "../../assets/css/var.less";
.MallShow{
  width: 100%;
  .logo{
    display: block;
    margin: -10px auto 30px;
    text-align: right;
    user-select:none;
    vertical-align: middle;
    img{
      display: inline-block;
      width: 33.3%;
    vertical-align: middle;
    }
    .searchBox{
      display: inline-block;
      width: 33.3%;
      vertical-align: middle;
      text-align: left;
      .TipsInput{
        margin-left: 30px;
      }
      .icon-search{
        font-size: 24px;
        font-weight: bold;
        color:@thirdColor;
        cursor: pointer;
        position: relative;
        top: 4px;
      }
    }
  }
  .typeList{
    width: 100%;
    text-align: center;
    background-color: white;
    li{
      display: inline-block;
      width: 60px;
      margin: 0 50px;
      text-align: center;
      height: 30px;
      line-height: 20px;
      font-weight: 600;
      font-size: 14px;
      cursor: pointer;
    }
    .selected{
      color:@thirdColor;
      border-bottom: 3px solid @thirdColor;
    }
  }
    .fixedNavContainer{
    text-align: left;
    .fixedLeft{
      display: inline-block;
      vertical-align: middle;
      width: 15%;
      height: 100%;
      font-size:30px;
      color:@thirdColor;
      user-select: none;
      line-height: 64px;
      text-align: center;
      cursor: pointer;
    }
    .fixedRight{
      width: 70%;
      height: 100%;
      display: inline-block;
      vertical-align: middle;
      li{
        display: inline-block;
        width: 60px;
        margin: 0 30px;
        text-align: center;
        height: 30px;
        line-height: 20px;
        font-weight: 600;
        font-size: 14px;
        cursor: pointer;
        position: relative;
        top: 4px;
      }
      .selected{
        color:@thirdColor;
        border-bottom: 3px solid @thirdColor;
      }
    }
  }
}
</style>
