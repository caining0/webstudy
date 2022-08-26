## Vue 基础学习

### Vue 是什么
Vue (发音为 /vjuː/，类似 view) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。
<img src="http://p0.qhimg.com/t018aaf686e4eba1701.png" alt="image-20220328153653919" style="zoom:50%;" />
### 怎么理解渐进式
- 可轻量
- 可重型项目（可扩展）
### 谁开发的
尤雨溪，前端框架Vue.js的作者，HTML5版Clear的打造人，独立开源开发者。曾就职于Google Creative Labs和Meteor Development Group。由于工作中大量接触开源的JavaScript项目，最后自己也走上了开源之路，现全职开发和维护Vue.js
<img src="https://avatars.githubusercontent.com/u/499550?v=4" alt="image-20220328153653919" style="zoom:50%;" />
### 特征
 - 1.采用 【组件化】 模式，提高代码复用率、让代码更好维护，每一个.vue文件就是一个组件、                  
- 2.声明式编码，无需直接操作DOM，省去了getElementById直接操作dom，提高开发效率
- MVVM

### 模板语法
#### 插值语法
- 插值【标签体】内容
```html
<span>Message: {{ msg }}</span>
```
#### 指令语法
- 指令语法用于【解析标签】包括标签属性、标签体内容、绑定事件
```html
    <a href="address">【Vue 官网】-错误1 </a>
    <a href="{{address}}">【Vue 官网】-错误2 </a>
    <a v-bind:href="address">【Vue 官网】-正确1 </a>
    <a :href="address">【Vue 官网】-正确2 </a>
    <!-- <a v-model="address">【Vue 官网】-错误 </a> -->

    /Users/cnn/Desktop/web-study/vue-first-demo/src/components/day_01_Ref.vue
  50:9  error  'v-model' directives aren't supported on <a> elements
```
  <img src="http://p0.qhimg.com/t011b63c37ab895f691.png
" alt="image-20220328153653919" style="zoom:50%;" />
- v-bind:  =====> : 
- Vue 中很多指令
  <img src="http://p0.qhimg.com/t0160c3d30561ee7a59.png" alt="image-20220328153653919" style="zoom:50%;" />
  - v-bind : 单向绑定 缩写`:` 或者 `.`
  - v-model :双向绑定/仅限 `<input>` `<select>` `<textarea>` `components`
  - v-on ：绑定事件 缩写 `@`
    - .stop ——调用 `event.stopPropagation()`。
    - .prevent ——调用 `event.preventDefault()`。
    - .capture ——在捕获模式添加事件监听器。 事件是由外传内，再冒泡 是冒泡之后执行，capture 可以阻挡内传
    - .self ——只有事件从元素本身发出才触发处理函数。
    - .{keyAlias} ——只在某些按键下触发处理函数。
    - .once ——最多触发一次处理函数。
    - .left ——只在鼠标左键事件触发处理函数。
    - .right ——只在鼠标右键事件触发处理函数。
    - .middle ——只在鼠标中键事件触发处理函数。
    - .passive ——通过 { passive: true } 附加一个 DOM 事件
- 事件流 捕获和冒泡
- v-if: 基于表达式值的真假性，来条件性地渲染元素或者模板片段
- v-text : 更新元素的文本内容
```html
<span v-text="msg"></span>
<!-- 等同于 -->
<span>{{msg}}</span>
```
- v-html : 更新元素的 innerHTML
```json
{
    "data": "<form id=\"Ordersubmit\" name=\"Ordersubmit\" action=\"https://pay.china-m2m.com/Pay/Alipay/index.html\" method=\"post\"><input type=\"hidden\" name=\"payStr\" value=\"dXdNa3JVdVg4L1k1bnRXeVFKcXlGYS9YMnJwdlYvU0k4NUl6NE5FTHpsVmE3dUlldHZwUE4wL2hBREtkcDBSbms1111123123123kUwRGFQUy9CNnVaZTV2Y0RUMTYvSE5ZRWZRVTVVL1Ntb2RtdE0vbEtDc2VXL0YrbTRvQ1IyZW4wRlpYMU9TWWdZNUFUb0tBUERKSTRjYUQvc2pFNTVabnpRSDdMS3FIWlE9PQ==\"/></form><script>document.forms['Ordersubmit'].submit();</script>",
    "extra": {
        "call_model": "form",
        "order_no": "YL20111111111111"
    },
    "msg": "操作成功",
    "state": 1
}
```

