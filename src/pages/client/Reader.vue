<template>
  <div id="area">





  </div>
</template>

<script>
import {getCollects} from '../../api/client';
import Epub from 'epubjs'
import {mapState} from "vuex";
global.ePub = Epub;
export default {
  name: 'Reader',
  computed: {
    ...mapState([
      'clientToken'
    ]),
  },
  data(){
  	return{
  		collects:[]
  	}
  },
  mounted(){

    // 生成Book对象
    this.book = new Epub("api/static/1.txt")
    // 通过Book.renderTo生成Rendition对象
    this.rendition = this.book.renderTo('read', {
      width: window.innerWidth,
      height: window.innerHeight,
      // 兼容iOS
      method: 'default'
    })
    // 通过Rendtion.display渲染电子书
    this.rendition.display()
    // 获取Theme对象
    this.themes = this.rendition.themes
    // 设置默认字体
    //this.setFontSize(this.defaultFontSize)
    // 注册主题
   // this.registerTheme()
    // 设置默认主题
    //this.setTheme(this.defaultTheme)
    // Book对象的钩子函数ready
    this.book.ready.then(() => {
      this.navigation = this.book.navigation
      // 生成Locations对象
      return this.book.locations.generate()
    }).then(result => {
      // 保存locations对象
      this.locations = this.book.locations
      // 标记电子书为解析完毕状态
      this.bookAvailable = true
    })
   /* var book = ePub("/static/1.txt");
    var rendition = book.renderTo("area", {width: 600, height: 400});
    var displayed = rendition.display();*/
  },
  methods:{
    navTo(route){
      this.$router.push(route);
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
