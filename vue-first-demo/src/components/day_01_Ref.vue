<template>
  <div>
    <van-divider :style="{ color: '#1989fa', borderColor: '#1989fa', padding: '0 16px' }">QiShare</van-divider>
    <van-collapse v-model="activeNames"  >
      <van-collapse-item title="Vue 是什么" name="1">
        aaa
      </van-collapse-item>
      <van-collapse-item title="谁开发的？" name="2">
        尤雨溪，前端框架Vue.js的作者，HTML5版Clear的打造人，独立开源开发者。曾就职于Google Creative Labs和Meteor Development Group。由于工作中大量接触开源的JavaScript项目，最后自己也走上了开源之路，现全职开发和维护Vue.js
      </van-collapse-item>
      <van-collapse-item title="Vue 特点" name="3">
        - 1.采用 【组件化】 模式，提高代码复用率、让代码更好维护，每一个.vue文件就是一个组件、                  
        - 2.声明式编码，无需直接操作DOM，省去了getElementById直接操作dom，提高开发效率
        - 声明式：
        

      </van-collapse-item>
    </van-collapse>
  <v-md-editor id="mark" v-model="markdown2" mode="preview" ></v-md-editor>
  </div>
  <van-divider>文字</van-divider>
<div>
  <van-image
    width="100"
    height="100"
    src="https://avatars.githubusercontent.com/u/499550?v=4"
  />
<a href="https://github.com/yyx990803">尤雨溪Github</a>

</div>

  <van-divider :style="{ color: '#1989fa', borderColor: '#1989fa', padding: '0 16px' }">Demo</van-divider>

  <div class="demo_show">
    <span>例子</span>
    <ul id="list">
        <li v-for="(p,index) in persons" :key="index">
        {{p.name}} - {{p.age}}</li>
    </ul>
  </div>
</template>
<script>


import mdFile from '../../public/markdown/day01.md'

export default {
  name: "day_01_Ref",
  data() {
    console.log("cnn+day_01", this);
    return {
      markdown2: "",
      persons: [
        {name : "张三",age : 18},
        {name : "李四",age : 18},
        {name : "王五",age : 18},
        {name : "赵六",age : 18},
        {name : "李四",age : 20}],
      activeNames: [],
    };
  },
  mounted() {
    this.getMDFile()
  },
  methods: {
    getMDFile() {
      this.markdown2 = mdFile
    },
  }
};
</script>
<style>
.demo_show {
  background-color: blanchedalmond;
  width: 500px;
  height: 500px;
  padding: 16px;
  text-align: start;;
}
</style>
