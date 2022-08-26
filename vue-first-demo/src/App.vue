<template >
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->

<div class ='root'>
 <van-divider >Vue基础学习大纲</van-divider>
<van-checkbox-group  ref ='group'  direction="horizontal" v-model="showWhat" @change="handleChange">
  <van-checkbox name="Day_01_Ref">{{arr_days[0]}}</van-checkbox>
  <van-checkbox name="Day_02">{{arr_days[1]}}</van-checkbox>
  <van-checkbox name="Day_03">{{arr_days[2]}}</van-checkbox>
  <van-checkbox name="Day_04">{{arr_days[3]}}</van-checkbox>
  <van-checkbox name="Day_05">{{arr_days[4]}}</van-checkbox>
  <van-checkbox name="Day_06">{{arr_days[5]}}</van-checkbox>
  <van-checkbox name="Day_07">{{arr_days[6]}}</van-checkbox>
  <van-checkbox name="Day_08">{{arr_days[7]}}</van-checkbox>
  <van-checkbox name="Day_09">{{arr_days[8]}}</van-checkbox>
  <van-checkbox name="Day_10">{{arr_days[9]}}</van-checkbox>
  <van-checkbox name="Day_11">{{arr_days[10]}}</van-checkbox>
  <van-checkbox name="Day_12">{{arr_days[11]}}</van-checkbox>
  <van-checkbox name="Day_13">{{arr_days[12]}}</van-checkbox>

</van-checkbox-group>

  <Day_01_Ref v-if="this.showWhat[0]==='Day_01_Ref'"></Day_01_Ref>
  <Day_02  v-if="this.showWhat[0]==='Day_02'" ></Day_02>
  <Day_03  v-if="this.showWhat[0]==='Day_03'"></Day_03>
  <Day_04  v-if="this.showWhat[0]==='Day_04'"></Day_04>
  <Day_05  v-if="this.showWhat[0]==='Day_05'"></Day_05>
  <Day_06  v-if="this.showWhat[0]==='Day_06'"></Day_06>
  <Day_07  v-if="this.showWhat[0]==='Day_07'"></Day_07>
  <Day_08  v-if="this.showWhat[0]==='Day_08'"></Day_08>
  <Day_09  v-if="this.showWhat[0]==='Day_09'"></Day_09>
  <Day_10 v-if="this.showWhat[0]==='Day_10'"></Day_10>
  <Day_11 v-if="this.showWhat[0]==='Day_11'"></Day_11>
  <Day_12 v-if="this.showWhat[0]==='Day_12'"></Day_12>
  <Day_13 v-if="this.showWhat[0]==='Day_13'"></Day_13>


   <div>
        <span>资源：</span>
        <a href="https://cn.vuejs.org/">【Vue 官网】 </a>
        <a href="https://github.com/yyx990803">【尤雨溪】</a>
    </div>
</div>
</template>

<script>
import Day_01_Ref from "./components/day_01_Ref.vue";
import Day_02 from "./components/day_02.vue";
import { ref } from 'vue';
import Day_03 from './components/day_03.vue';
import Day_04 from './components/day_04.vue';
import Day_05 from './components/day_05.vue';
import Day_06 from './components/day_06.vue';
import Day_08 from './components/day_08.vue';
import Day_09 from './components/day_09.vue';
import Day_10 from './components/day_10.vue';
import Day_11 from './components/day_11.vue';
import Day_12 from './components/day_12.vue';
import Day_13 from './components/day_13.vue';
import Day_07 from './components/day_07.vue';


export default {
  name: "App",
  components: {
    Day_01_Ref,
    Day_02,
    Day_03,
    Day_04,
    Day_05,
    Day_06,
    Day_08,
    Day_09,
    Day_10,
    Day_11,
    Day_12,
    Day_13,
    Day_07,
},
  setup() {
    const showWhat = ref(['Day_01_Ref']);
    return { showWhat };
  },
  data(){
    return {
      arr_days: [
        "Vue笔记-指令与事件修饰",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定",
        "数据绑定"
      ]
    }
  },
  methods: {

    handleChange(val){
      if(this.showWhat.length==2){
        this.$refs.group.toggleAll(false);
        this.showWhat[0]=val[1]
      }

    }
  },
};
</script>
<style >
.root{
  background-color: white;
  padding: 16px;
}
</style>