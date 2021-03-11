# VueJS

## 1. 前端简介

Vue 只负责视图层的框架（HTML + CSS + JS）(SoC：关注点分离原则)

网络通信：axios

页面跳转：vue-router

状态管理：vuex

Vue-UI：Ice

生成 CSS：LESS

JS ES6 -> ES5：webpack

JS 框架

- AngularJS：MVVM（MVC）框架
- React：虚拟 DOM，JSX
- Vue：MVVC + 虚拟 DOM
- Axios：通信框架

UI 框架

- Ant-Design
- Element-UI
- Bootstrap
- AmazeUI

JS 构建工具

- Babel
- WebPack

三端统一 Hybrid App

- 云打包：HBuild -> HBuild，DCloud
- 本地打包：Cordova

后端技术

- Express：NodeJS
- Koa：Express 简化版
- NPM：项目管理工具
- YARN：NPM 的替代方案

主流前端框架 Vue.js

- iView
- ElementUI
- ICE

## 2. MVVM

- View（DOM）

  - HTML
  - CSS
  - Template

  View <-- 双向数据绑定 --> ViewModel

- ViewModel 

  - JavaScript
  - Runtime
  - Compiler

  View <-- Ajax JSON --> Model

- Model

  - JS 对象

## 3. Vue 简单控制语句

CDN

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

双向绑定 1

```html
<body>
<div id="app1">
    {{message}}
</div>
<script>
    let vm = new Vue({
        el: "#app1",
        data: {
            message: "hello vue"
        }
    });
</script>
</body>
```

双向绑定 2

```html
<body>
<div id="app">
    <span v-bind:title="message">
        Move on to show message
    </span>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: "V-bind"
        }
    })
</script>
</body>
```

if 使用

```html
<body>
<div id="app">
    <h1 v-if="type==='A'">Type A</h1>
    <h1 v-else-if="type==='B'">Type B</h1>
    <h1 v-else>Others</h1>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            type: 'A'
        }
    })
</script>
</body>
```

for 使用

```html
<body>
<div id="app">
    <h1 v-for="(item,index) in items">
        {{item.message+'--'+index}}
    </h1>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            items: [{message: 'A'},{message: 'B'},{message: 'C'}]
        }
    })
</script>
</body>
```

事件监听

```html
<body>
<div id="app">
    <button v-on:click="hello()">Click Me</button>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: "hello"
        },
        methods: {
            hello: function () {
                alert(this.message);
            }
        }
    })
</script>
</body>
```

双向绑定

```html
<body>
<div id="app">
    <input type="text" v-model="message"> {{message}}<br>
    <hr>
    <input type="radio" name="sex" value="homme" v-model="sex">Homme<br>
    <input type="radio" name="sex" value="femme" v-model="sex">Femme<br>
    <input type="radio" name="sex" value="null" v-model="sex">Null<br>
    Resutlat : {{sex}}
    <hr>
    <select name="alphabet" id="alphabet" v-model="alphabet">
        <option> </option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select><br>
    Lettre: {{alphabet}}
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            message: "message",
            checked: false,
            sex: "",
            alphabet: "V"
        },

    })
</script>
```

组件

```html
<body>
<div id="app">
    <comp1 v-for="lang in items" v-bind:language="lang"></comp1>
</div>
<script>
    Vue.component("comp1",{
        props: ['language'],
        template: "<li>{{language}}</li>"
    });
    let vm = new Vue({
        el: "#app",
        data: {
            items: ["Java", "MySQL", "PHP"]
        }
    })
</script>
</body>
```

## 4. 网络通信 Axios

- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API [JS 中链式编程]
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF （跨站请求伪造）

```html
<body>
<div id="vue" v-cloak>
    <div>{{info}}</div>
    <a v-bind:href="info.links[0].url">Click</a>
</div>
<script>
    let vm = new Vue({
        el: "#vue",
        data(){
            return {
                info:{
                    name: null,
                    age: 0,
                    adult: false,
                    address: {
                        num: 0,
                        name: null,
                        city: null,
                    },
                    links: [
                        {
                            name: null,
                            url: null
                        }
                    ]
                }
            }
        },
        mounted(){
            axios.get('data.json').then(response => (this.info = response.data));
        }
    })
</script>
</body>
```

## 5. 计算属性

用属性调用 computed 内部方法，方法内容刷新时刷新

```html
<body>
<div id="vue">
    <p>Current Time 1 {{currentTime1()}}</p>
    <p>Current Time 1 {{currentTime2}}</p>
</div>
<script>
    let vm = new Vue({
        el: "#vue",
        data: {
            message: "hello"
        },
        methods: {
            currentTime1: function () {
                return Date.now();
            }
        },
        computed: {
            currentTime2: function () {
                this.message;
                return Date.now();
            }
        }
    })
</script>
</body>
```

## 6. 内容分发 slot

```html
<body>
<div id="vue">
    <todo-list>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-item slot="todo-item" v-for="item in todoItems" :items="item"></todo-item>
    </todo-list>
</div>
<script>
    Vue.component("todo-list",{
        template: '\
            <div>\
                <slot name="todo-title"></slot>\
                <ul>\
                    <slot name="todo-item"></slot>\
                </ul>\
            </div>'
    })
    Vue.component("todo-title",{
        props: ['title'],
        template: '<div>{{title}}</div>'
    })
    Vue.component("todo-item",{
        props: ['items'],
        template: '<li>{{items}}</li>'
    })
    let vm = new Vue({
        el: "#vue",
        data: {
            title: "Book list",
            todoItems: ['Java','Linux','MySQL']
        }
    })
</script>
</body>
```

