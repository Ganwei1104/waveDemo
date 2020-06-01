<template>
  <div class="documentation">
    <div class="app-container documentation-container">
      <a
        class="document-btn"
        target="_blank"
        href="https://panjiachen.github.io/vue-element-admin-site/"
      >Documentation</a>
      <a
        class="document-btn"
        target="_blank"
        href="https://github.com/PanJiaChen/vue-element-admin/"
      >Github Repository</a>
      <a
        class="document-btn"
        target="_blank"
        href="https://panjiachen.gitee.io/vue-element-admin-site/zh/"
      >国内文档</a>
      <dropdown-menu :items="articleList" style="float:left;margin-left:50px;" title="系列文章"/>
    </div>
  <div class="app-container documentation-container">
    <a
      class="document-btn"
      target="_blank"
      href="https://panjiachen.github.io/vue-element-admin-site/"
    >Documentation</a>
    <a
      class="document-btn"
      target="_blank"
      href="https://github.com/PanJiaChen/vue-element-admin/"
    >Github Repository</a>
    <a
      class="document-btn"
      target="_blank"
      href="https://panjiachen.gitee.io/vue-element-admin-site/zh/"
    >国内文档</a>
    <dropdown-menu :items="articleList" style="float:left;margin-left:50px;" title="系列文章" />
    <a
      class="document-btn"
      target="_blank"
      href="https://panjiachen.github.io/vue-element-admin-site/zh/job/"
    >内推招聘</a>
     <!--轮播图片测试-->
    </div>
    <div class="carousel-card">
      <el-carousel :width="width" :height="height" @change="changeCard">
        <el-carousel-item v-for="(item,index) in carouselList" :key="index">
          <img class="carouselImg" :width="img_W" :height="img_H" :src="item.url" @load="onLoad">
        </el-carousel-item>
      </el-carousel>
    </div>
  </div>
</template>

<script>
import DropdownMenu from '@/components/Share/DropdownMenu'

export default {
  name: 'Documentation',
  components: { DropdownMenu },
  data() {
    return {
      width:'',
      height:'',
      img_W:'',
      img_H:'',
      carouselList:[
        {url:require('../../assets/img/0911-01.jpg')},
        {url:require('../../assets/img/0708_06.jpg')},
        {url:require('../../assets/img/111.jpg')},
        {url:require('../../assets/img/Img330116361.jpg')},
        {url:require('../../assets/img/1657d515aaa7ee7.jpg')},
      ],
      imgData:[],
      articleList: [
        { title: '基础篇', href: 'https://juejin.im/post/59097cd7a22b9d0065fb61d2' },
        { title: '登录权限篇', href: 'https://juejin.im/post/591aa14f570c35006961acac' },
        { title: '实战篇', href: 'https://juejin.im/post/593121aa0ce4630057f70d35' },
        { title: 'vue-admin-template 篇', href: 'https://juejin.im/post/595b4d776fb9a06bbe7dba56' },
        { title: 'v4.0 篇', href: 'https://juejin.im/post/5c92ff94f265da6128275a85' },
        { title: '自行封装 component', href: 'https://segmentfault.com/a/1190000009090836' },
        { title: '优雅的使用 icon', href: 'https://juejin.im/post/59bb864b5188257e7a427c09' },
        { title: 'webpack4（上）', href: 'https://juejin.im/post/59bb864b5188257e7a427c09' },
        { title: 'webpack4（下）', href: 'https://juejin.im/post/5b5d6d6f6fb9a04fea58aabc' }
      ]
    }
  },
  created(){
    
  },
  methods:{
      changeCard(currentTndex){
        for(let i=0;i<=this.imgData.length;i++){
            if(currentTndex === i){
              this.width = this.img_W = this.imgData[i].width;
              this.height = this.img_H = this.imgData[i].height;
              console.log('----------------',this.width,this.height);
            }
          }
      },
      onLoad(e){
        const img = e.target;
        let width = 500;
        let height = '';
        let bili = Number(width)/Number(img.width);
        width = width + 'px';
        height = parseInt(Number(img.height)*bili) + 'px';
        let list = {
          width:width,
          height:height
        }
        this.imgData.push(list);
        this.width = this.imgData[0].width;
        this.height = this.imgData[0].height;
    },
  }
}
</script>

<style lang="scss" scoped>
  .documentation{
    .documentation-container {
      margin: 50px;
      .document-btn {
        float: left;
        margin-left: 50px;
        display: block;
        cursor: pointer;
        background: black;
        color: white;
        height: 60px;
        width: 200px;
        line-height: 60px;
        font-size: 20px;
        text-align: center;
      }
    }
    .carousel-card{
      margin: 100px;
      width: 500px;
    }
  }
.documentation-container {
  margin: 50px;
  display: flex;
  flex-wrap: wrap;
  .document-btn {
    margin-left: 50px;
    display: block;
    cursor: pointer;
    background: black;
    color: white;
    height: 60px;
    width: 200px;
    margin-bottom: 16px;
    line-height: 60px;
    font-size: 20px;
    text-align: center;
  }
}
</style>
