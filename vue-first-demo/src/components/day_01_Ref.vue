<template>
  <van-divider
    :style="{ color: '#1989fa', borderColor: '#1989fa', padding: '0 16px' }"
    >Demo</van-divider
  >
   <van-collapse v-model="activeNames">
      <van-collapse-item title="Vue 基础" name="1">
        <v-md-editor id="mark" v-model="markdown2" mode="preview"></v-md-editor>
      </van-collapse-item>
    </van-collapse>

  <div class="demo_show">
    <h2>例子1 v-for</h2>
    <ul id="list">
      <li v-for="(p, index) in persons" :key="index">
        {{ p.name }} - {{ p.age }}
      </li>
    </ul>
    <h2>例子2 插值语法</h2>
    <span>你好，{{ qi_share }}</span>
    <h2>例子3 指令语法</h2>
    <div class="div_column">
      <a href="address">【Vue 官网】-错误1 </a>
      <a href="{{address}}">【Vue 官网】-错误2 </a>
      <a v-bind:href="address">【Vue 官网】-正确1 </a>
      <a :href="address">【Vue 官网】-正确2 </a>
      <!-- <a v-model="address">【Vue 官网】-错误 </a> -->
    </div>

    <van-divider />
    <input placeholder="v-model" v-model="qi_share" />
    <input placeholder="v-bind:value" v-bind:value="qi_share" />
    <input placeholder=":value" :value="qi_share" />
    <input
      placeholder="v-bind模拟v-model"
      :value="qi_share"
      @input="inputChange"
    />

    <h2>例子4 点击层级和 事件修饰符</h2>

    <div  class="div_row">
      <div @scroll="wheel" id="id1" @click.self="clickWhat(1)" class="demo_show list">
        div1
        <div v-for="index in 100" :key="index" id="id2" @click="clickWhat(2)" class="demo_show show2">{{index}}</div>
      </div>

      <div id="id3" @click.self="clickWhat(1)" class="demo_show show1">
        div1
        <button id="id4" @click="clickWhat(2)" class="demo_show show2">
          but
        </button>
      </div>
      <!-- <ul @scroll="wheel" class="demo_show list">
        <li v-for="index in 100" :key="index">{{ index }}</li>
      </ul> -->
    </div>


    <van-divider />
    <a href="https://www.google.com" @click.prevent="showDialog">点我</a>
    <button href="https://www.google.com" @click.once="showDialog">
      只触发一次
    </button>
  </div>
  <van-divider />
  <div v-html="testJson" class="demo_show show1"></div>




  <div>
    <van-divider
      :style="{ color: '#1989fa', borderColor: '#1989fa', padding: '0 16px' }"
      >QiShare</van-divider
    >

 
  </div>
</template>
<script>
import mdFile from "../../public/markdown/day01.md";

export default {
  name: "day_01_Ref",
  setup(){
    const testJson = require('../test/test.json').data
      console.log("------>testJson" + testJson);

    return {
      testJson
    }

  },
  data() {
    console.log("data->this", this);
    return {
      markdown2: "",
      persons: [
        { name: "张三", age: 18 },
        { name: "李四", age: 18 },
        { name: "王五", age: 18 },
        { name: "赵六", age: 18 },
        { name: "李四", age: 20 },
      ],
      qi_share: "QiShare",
      address: "https://cn.vuejs.org/",
      activeNames: [],
    };
  },
  mounted() {
    this.getMDFile();
  },
  methods: {
    getMDFile() {
      this.markdown2 = mdFile;
    },
    clickWhat(a) {
      console.log("------>" + a);
    },

    wheel() {
      for (let i = 0; i < 100; i++) {
        console.log("😈");
      }
      console.log("累啊💦");
    },

    showDialog() {
      alert("123123123");
    },
    inputChange(event,b,c,d) {
      console.log(event,b,c,d);
      this.qi_share = event.target.value;
    },
  },
};
</script>
<style>
.demo_show {
  background-color: blanchedalmond;
  width: 500px;
  padding: 16px;
  text-align: start;
}
.demo_show.show1 {
  width: 100px;
  height: 100px;
  padding: 20px;
  margin-left: 20px;
  background-color: ghostwhite;
}
.demo_show.show2 {
  width: 50px;
  height: 50px;
  background-color: red;
}
.demo_show.list {
  width: 100px;
  height: 100px;
  padding: 20px;
  margin-left: 20px;
  background-color: ghostwhite;
  overflow: auto;
}
.div_row {
  display: flex;
  flex-direction: row;
  background-color: aquamarine;
}
.div_column {
  display: flex;
  flex-direction: column;
  background-color: ivory;
}
</style>
