### DataWhale —— 推荐系统学习三(前后端交互)

### 1. Web前端

#### 1.1 什么是Web

 Web（World Wide Web）即全球广域网，也称为万维网，它是一种基于超文本和HTTP的、全球性的、动态交互的、跨平台的分布式图形信息系统。是建立在Internet上的一种网络服务，为浏览者在Internet上查找和浏览信息提供了图形化的、易于访问的直观界面，其中的文档及超级链接将Internet上的信息节点组织成一个互为关联的网状结构。

Web前端主要是通HTML,CSSJS,ajax,DOM等前端技术,实现网站在客服端的正确显示及交互功能。

#### 1.2 Web标准构成

主要包括结构（Structure）、表现（Presentation）和行为（Behavior）三个方面。

- **结构标准**：结构用于对网页元素进行整理和分类，对于网页来说最重要的一部分 。通过对语义的分析，可以对其划分结构。具有了结构的内容，将更容易阅读.
- **表现标准**：表现用于设置网页元素的版式、颜色、大小等外观样式，主要指的是CSS 。为了让网页能展现出灵活多样的显示效果.
- **行为标准**：行为是指网页模型的定义及交互的编写 。使用户对网页进行操作，网页可以做出响应性的变化。

总的来说，

- Web标准有三层结构，分别是结构（HTML）、表现（CSS）和行为（JS）
- 结构类似人的身体， 表现类似人的着装， 行为类似人的行为动作
- 理想状态下，他们三层都是独立的， 放到不同的文件里面

#### 1.3 JS介绍

##### 1.3.1JS组成

JS由以下三部分构成：

- **ECMAScript**： 是由ECMA 国际（ 原欧洲计算机制造商协会）进行标准化的一门编程语言，这种语言在万维网上应用广泛，它往往被称为 JavaScript或 JScript，但实际上后两者是 ECMAScript 语言的实现和扩展。
- **DOM**：文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过 DOM 提供的接口可以对页面上的各种元素进行操作（大小、位置、颜色等）
- **BOM**：浏览器对象模型(Browser Object Model，简称BOM) 是指浏览器对象模型，它提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等。
- ![JS三大部分构成](DataWhale —— 推荐系统学习三(前后端交互).assets/68747470733a2f2f67697465652e636f6d2f7468654e657665724c656d6f6e2f6e6577732d696d672f7261772f6d61737465722f696d672f6a73312e706e67.png)

##### 1.3.2 书写位置

**1.行内式**

```
<input type="button" value="点我试试" onclick="alert('Hello World')" />
```

- 可以将单行或少量 JS 代码写在HTML标签的事件属性中（以 on 开头的属性），如：onclick；
- 可读性差， 在HTML中编写JS大量代码时，不方便阅读；
- 引号易错，引号多层嵌套匹配时，非常容易弄混；

**2.内嵌式**

```
<script>
    alert('Hello  World~!');
</script>
```

- 可以将多行JS代码写到 script 标签中

**3.外部JS文件**

```
<script src="myScript.js"></script>
//myScript.js文件内容
function myFunction()
{
    document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
}
```

- 利于HTML页面代码结构化，把大段 JS代码独立到 HTML 页面之外，既美观，也方便文件级别的复用
- 引用外部 JS文件的 script 标签中间不可以写代码
- 适合于JS 代码量比较大的情况

参考链接：

https://www.runoob.com/js/js-tutorial.html

https://www.w3school.com.cn/js/index.asp

__ __

### 2. Vue简介

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

- Vue.js优点：
  - 体积小，压缩后33k
  - 更高的运行效率：基于虚拟dom，预先通过`JavaScript`进行各种计算，把最终的`DOM`操作计算出来并优化的技术
  - 双向数据绑定：开发者不用种子去操作`dom`对象，可以投入更多的精力到业务逻辑上
  - 生态丰富，学习成本低

- 参考：
  - [Vue3.js官方介绍](https://v3.cn.vuejs.org/guide/introduction.html#vue-js-%E6%98%AF%E4%BB%80%E4%B9%88)
  - [介绍 — Vue.js (vuejs.org)](https://cn.vuejs.org/v2/guide/)

#### 2.1安装

##### 2.1.1 通过<script> 标签引入 

直接下载并用`<script>`标签引入，`Vue`会注册为全局变量。

- **开发版本**：https://cn.vuejs.org/js/vue.js
- **生产版本**：https://cn.vuejs.org/js/vue.min.js  比较小，忽略了警告

##### 2.1.2 通过CDN安装

- **制作原型或学习**：

```vue
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
```

- **用于生产环境**：

```vue
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14"></script>
```

- **使用原生 ES Modules**：

```vue
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.esm.browser.js'
</script>
```

##### 2.1.3 通过NPM安装

在用 Vue 构建大型应用时推荐使用 NPM 安装。NPM 能很好地和诸如 webpack或 Browserify模块打包器配合使用。同时 Vue 也提供配套工具来开发单文件组件。

 由于 npm 安装速度慢，也可使用 cnpm安装，安装使用介绍参照：[使用淘宝 NPM 镜像](https://www.runoob.com/nodejs/nodejs-npm.html#taobaonpm)。

1.npm 版本需要大于 3.0，如果低于此版本需要升级它：

```vue
# 查看版本
$ npm -v
2.3.0

#升级 npm
cnpm install npm -g

# 升级或安装 cnpm
npm install cnpm -g
```

2.安装vue

```vue
# 使用npm安装
$ cnpm install vue

# 使用cnpm安装
$ cnpm install vue
```

##### 2.1.4 通过命令行工具 (CLI)安装

**注：CLI 工具假定用户对 Node.js 和相关构建工具有一定程度的了解。如果你是新手，我们强烈建议先在不用构建工具的情况下通读[指南](https://v3.cn.vuejs.org/guide/introduction.html)，在熟悉 Vue 本身之后再使用 CLI。**

 Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

- 通过 `@vue/cli` 实现的交互式的项目脚手架。

- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。

- 一个运行时依赖 (

  ```
  @vue/cli-service
  ```

  )，该依赖：

  - 可升级；
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。

- 一个丰富的官方插件集合，集成了前端生态中最好的工具。

- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

 Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject

以下是Vue3的CLI安装

对于 Vue 3，你应该使用 `npm` 上可用的 Vue CLI v4.5 作为 `@vue/cli`。要升级，你应该需要全局重新安装最新版本的 `@vue/cli`：

```bash
yarn global add @vue/cli
# 或
npm install -g @vue/cli
```

然后在 Vue 项目中运行：

```bash
vue upgrade --next
```

#### 2.2 创建一个Vue实例

##### 2.2.1 语法格式

每个 Vue 应用都需要通过实例化 Vue 来实现

```js
var vm = new Vue({

})
```

虽然没有完全遵循 [MVVM 模型](https://zh.wikipedia.org/wiki/MVVM)，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 `vm` (ViewModel 的缩写) 这个变量名表示 Vue 实例

一个 Vue 应用由一个通过 `new Vue` 创建的**根 Vue 实例**，以及可选的嵌套的、可复用的组件树组成。举个例子，一个 todo 应用的组件树可以是这样的：

```
根实例
└─ TodoList
   ├─ TodoItem
   │  ├─ TodoButtonDelete
   │  └─ TodoButtonEdit
   └─ TodoListFooter
      ├─ TodosButtonClear
      └─ TodoListStatistics
```

我们会在稍后的[组件系统](https://cn.vuejs.org/v2/guide/components.html)章节具体展开。不过现在，你只需要明白所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。



example:

```vue
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title></title>
 <!-- 引入Vue,可以实例化Vue -->
<script src="vue.js" type="text/javascript" charset="utf-8"></script>
</head>
<body>
	<div id="app">
	  {{ message }} {{name}}
	</div>
	
	<script type="text/javascript">
	var app = new Vue({
		el: '#app', // 这里是查找id
		data: {    // 定义里面的属性
			message: 'Hello Vue!',
			name : "Vue"
		}
	});
	</script>

</body>
</html>

```

- **data** ：定义属性，实例中有2个属性分别为：message、data。
- **methods** ：定义的函数，可以通过 return 来返回函数值。
- **{{ }}** ：输出对象属性和函数返回值。

##### 2.2.2 定义数据对象

当一个 Vue 实例被创建时，它将 `data` 对象中的所有的 property 加入到 Vue 的**响应式系统**中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```Vue
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 获得这个实例上的 property
// 返回源数据中对应的字段
vm.a == data.a // => true

// 设置 property 也会影响到原始数据
vm.a = 2
data.a // => 2

// ……反之亦然
data.a = 3
vm.a // => 3
```

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 `$`，以便与用户定义的 property 区分开来。例如：

```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

##### 2.2.3 实例生命周期钩子

详情查看Vue生命周期钩子API

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

注：生命周期函数不能使用 `-->` 函数。不要在选项 property 或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

定义之后，运行可以在`console`中查看生命周期执行函数顺序<img src="https://camo.githubusercontent.com/6a505edb0af50307f442c12b418e65a7bbab42e650b1e2b0db6248fd0787c327/68747470733a2f2f67697465652e636f6d2f7468654e657665724c656d6f6e2f6e6577732d696d672f7261772f6d61737465722f696d672f7675652e706e67">

> beforeCreate

 在实例初始化之后,进行数据侦听和事件/侦听器的配置之前同步调用。

 此时组件的选项对象还未创建，el 和 data 并未初始化，因此无法访问methods， data， computed等上的方法和数据。

> created

 在实例创建完成后被立即同步调用。

 实例已完成对选项的处理，以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 $el property 目前尚不可用。

 在这一步中可以调用methods中的方法，改变data中的数据，并且修改可以通过vue的响应式绑定体现在页面上，获取computed中的计算属性等等，通常我们可以在这里对实例进行预处理。但需要注意的是，这个周期中是没有什么方法来对实例化过程进行拦截的，因此假如有某些数据必须获取才允许进入页面的话，并不适合在这个方法发请求，建议在组件路由钩子beforeRouteEnter中完成

> beforeMount

 在挂载开始之前被调用：相关的 render 函数首次被调用（虚拟DOM）。

 实例已完成以下的配置： 编译模板，把data里面的数据和模板生成html，完成了el和data 初始化，但此时还没有挂在html到页面上。

> mounted

 实例被挂载后调用，这时 el 被新创建的 vm.$el 替换了。

 模板中的HTML渲染到HTML页面中，此时一般可以做一些ajax操作，**mounted只会执行一次**。

 但mounted 不会保证所有的子组件也都被挂载完成。如果希望等到整个视图都渲染完毕再执行某些操作，可以在 mounted 内部使用 vm.$nextTick：

```
mounted: function () {
  this.$nextTick(function () {
    // 仅在整个视图都被渲染之后才会运行的代码
  })
}
//生命周期钩子的 this 上下文指向调用它的 Vue 实例。
```

> beforeUpdate

 在数据发生改变后，DOM 被更新之前被调用。

 适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。可以在该钩子中进一步地更改状态，不会触发附加地重渲染过程.

> updated

 在数据更改导致的虚拟 DOM 重新渲染和更新完毕之后被调用。

 当这个钩子被调用时，组件 DOM 已经更新。，所以可以执行依赖于DOM的操作，然后在大多是情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。

 但updated 不会保证所有的子组件也都被重新渲染完毕。如果希望等到整个视图都渲染完毕，可以在 updated 里使用 vm.$nextTick：

```
updated: function () {
  this.$nextTick(function () {
    //  仅在整个视图都被重新渲染之后才会运行的代码     
  })
}
```

> beforeDestroy

 实例销毁之前调用。在这一步，实例仍然完全可用。

 这一步还可以用this来获取实例，一般用来做一些重置的操作，比如清除掉组件中的定时器和监听的dom事件。

> destroyed

 实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

参考链接：

https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks

https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram

https://vuejs.org/v2/api/index.html#Options-Lifecycle-Hooks

https://www.jianshu.com/p/672e967e201c

https://blog.csdn.net/haochangdi123/article/details/78358895

##### 2.2.4 创建一个Vue项目

> 安装vue CLI

 Vue CLI 的包名称由 `vue-cli` 改成了 `@vue/cli`。 如果已经全局安装了旧版本的 `vue-cli` (1.x 或 2.x)，需要先通过 `npm uninstall vue-cli -g` 或 `yarn global remove vue-cli` 卸载它。

1.安装新的包

```shell
npm install -g @vue/cli

# 或者
yarn global add @vue/cli
```

![image-20211221193845608](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221193845608.png)

2.检查其版本是否正确

```
vue --version
```

![image-20211221194021578](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221194021578.png)

3.升级包

```shell
npm update -g @vue/cli

# 或者
yarn global upgrade --latest @vue/cli
```

> 创建Vue项目

通过`vue create`创建

```shell
vue create hello-world
```

**默认配置(不同Vue版本)**：包含了基本的 Babel + ESLint 设置。适合快速创建一个新项目的原型

**手动选择特性**：选取需要的特性。适合面向生产的项目

![image-20211221194148376](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221194148376.png)





进入项目具体路径

```shell
cd hello-world
```

下载依赖

```shell
npm install
```

启动运行项目

```shell
npm run serve 
```

项目打包

```shell
npm run build
```

![image-20211221195243883](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221195243883.png)

> 使用2.x 模板创建，旧版本

```shell
# 全局安装一个桥接工具
npm install -g @vue/cli-init

# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack vue_map_test
```



```shell
# 进入项目具体路径
cd vue_map_test

# 下载依赖
npm install

# 启动运行项目，默认为8080端口
npm run dev

# 项目打包
npm run build
```

#### 2.3 Vue项目目录——前端目录

创建目录

![image-20211221200028853](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221200028853.png)

```
├── v-proj
|	├── node_modules  	// 当前项目所有依赖，一般不可以移植给其他电脑环境
|	├── public			
|	|	├── favicon.ico	// 标签图标
|	|	└── index.html	// 当前项目唯一的页面
|	├── src
|	|	├── assets		// 静态资源img、css、js
|	|	├── components	// 小组件
|	|	├── App.vue		// 根组件
|	|	├── main.js		// 全局脚本文件（项目的入口）
|	|	└── router.js   // 路由脚本文件	
|	├── README.md
└	└── package.json  //配置文件,使用npm install安装
```

##### 2.3.1 Public

 可以理解为入口目录

###### 2.3.1.1 favicon.ico

用于作为缩略的网站标志，它显示位于浏览器的地址栏或者在标签上，用于显示网站的logo。

###### 2.3.1.2 index.html

首页入口文件，可以添加一些 meta 信息或统计代码。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Title</title>
</head>
<body style="background-color: #fff;">
    <div id="app" >

    </div>
</body>
</html>
```



##### 2.3.2 src

###### 2.3.2.1 assets

 放置一些资源文件。比如js 、css、image等。

###### 2.3.2.2 components

例如：

```
<template>
    <div class="test">
        
    </div>
</template>

<script>
    export default {
        name: "Test"
    }
</script>

<style scoped>

</style>
```



- **有且只有一个根标签**

- **<script> 必须将组件对象导出 export default {}**

- <style> 标签明确scoped属性，代表该样式只在组件内部起作用(样式的组件化)

- 

###### 2.3.2.3 App.vue

-  是**整个项目的入口文件**，相当于包裹整个页面的最外层的div。

- ```html
  <template>
    <div id="app">
        
      <router-view/>
    </div>
  </template>
  
  <script>
  export default {
    name: 'App'
    components:{
     
    },
  }
  </script>
  
  <style>
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
    }
  
  </style>
  ```

###### 2.3.2.4 main.js

 **是项目的主JS，全局的使用的各种变量、js、插件 都在这里引入 、定义**

```shell
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
    router,
    render: h => h(App)
}).$mount('#app')
```

###### 2.3.2.5 router.js

```html
// 导入VueRoute路由组件
import VueRouter from 'vue-router'


// 导入各个组件
import homePage from "./components/homePage.vue";
import test from "./components/test.vue";

// 创建路由对象
let routerObj = new VueRouter({
    routes: [
        {path: '/', component: homePage},
        {path: '/test', component: test},
    ],
});

// 把routerObj对象暴漏出去。main.js导入这个数据
export default routerObj
```

##### 2.3.3  package.json

 是整个项目用的到的所有的插件的json的格式，比如插件的名称、版本号。 当在项目里使用npm install 时 node 会自动安装文件里的所有插件。

- ```
  {
    "name": "test",
    "version": "0.1.0",
    "private": true,
    "scripts": {
      "serve": "vue-cli-service serve",
      "build": "vue-cli-service build",
      "lint": "vue-cli-service lint"
    },
    "dependencies": {
      "core-js": "^3.6.5",
      "vue": "^3.0.0"
    },
    "devDependencies": {
      "@vue/cli-plugin-babel": "~4.5.0",
      "@vue/cli-plugin-eslint": "~4.5.0",
      "@vue/cli-service": "~4.5.0",
      "@vue/compiler-sfc": "^3.0.0",
      "babel-eslint": "^10.1.0",
      "eslint": "^6.7.2",
      "eslint-plugin-vue": "^7.0.0"
    },
    "eslintConfig": {
      "root": true,
      "env": {
        "node": true
      },
      "extends": [
        "plugin:vue/vue3-essential",
        "eslint:recommended"
      ],
      "parserOptions": {
        "parser": "babel-eslint"
      },
      "rules": {}
    },
    "browserslist": [
      "> 1%",
      "last 2 versions",
      "not dead"
    ]
  }
  ```

- 

- 参考链接：

- https://www.runoob.com/vue3/vue3-directory-structure.html

- https://blog.csdn.net/weixin_41887155/article/details/107648969

- https://www.cnblogs.com/jhpy/p/11873270.html

- https://blog.csdn.net/chao2458/article/details/81284522

- 



### 3 前后端交互

#### 3.1 后端目录

```
news_rec_sys/
    conf/
    	dao_config.py
    controller/  
    dao/
    materials/
    	news_scrapy/
    	user_proccess/
    	material_proccess
    recpocess/
    	recall/
    	rank/
    	online.py
    	offline.py
    scheduler/
    server.py
```

- conf/dao_config.py 候选整体配置文件，其中包括数据库的配置、新闻类别的映射
- controller/: 项目中用于操作数据库的接口
- dao/: 项目实体类，对于数据库的表
- materials/: 项目的物料部分，在物料模块爬取过程中特意提到，处理用户画像和新闻画像
- recprocess/: 项目的推荐模块，主要包含召回和排序，以及一些线上服务和线下处理部分
- server.py 项目后端入口部分，主要包含项目整体的后端接口部分。包括后端IP的设定等
- scheduler: 项目的定时任务的脚本部分

在该项目中，**前端主要使用的是Vue框架+mint-ui，后端主要使用的是Flask+Mysql+Mongodb+Redis来完成的**，并且前后端采用分离的方式，通过**json的数据格式**进行数据传递。其中该项目后端的主要逻辑在在server.py中，其中主要包含用户注册，登录，推荐列表，热门列表，获取新闻详情页以及用户的行为等功能。接下来将主要按照这几部分详细的介绍一下前后端是如何进行交互

##### 3.1用户注册登录

为了能够对用户进行千人千面的推荐，因此需要每个使用该系统的人都需要明确先进行注册登入，为每个用户生成唯一的用户id，根据用户的历史行为，实现对用户进行个性化推荐的效果。

[server.py](https://github.com/datawhalechina/fun-rec/blob/master/codes/news_recsys/news_rec_server/server.py) 中的 `register()`函数：

1. `get_data()`读取前端传来的信息，解析**Json**格式。
2. 从`Mysql`表格中判断是否注册过该用户
3. 值得注意的是，为了防止并发问题导致用户id出现重复问题，因此这里采用了Twitter的雪花算法来为给每个用户生成一个唯一的id



server.py 中的 `login()`函数：

前端通过将输入的账号密码通过POST请求传给 /recsys/login，通过UserAction().user_is_exist()方法查询数据库中的用户名或者密码是否存在，其中1表示账号密码正确，2表示密码错误，0表示用户不存在



##### 3.2 推荐页列表

在项目样式展现的部分中，第一附图就是推荐页列表的样式，通过瀑布流的方式将新闻内容进行展现。

![image-20211221213251259](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221213251259.png)这个啥子意思？——老用户传过来的是一个无效字符串，新用户注册才会有字符串数字

部分的主要逻辑是前端通过请求 "/recsys/rec_list" 接口，后端通过前端传递过来的用户姓名，从数据库中获取用户id，再根据用户id去推荐服务(recsys_server)中获取到推荐列表



##### 3.3 热门推荐页

热门推荐页部分，前端通过请求'/recsys/hot_list'接口，通过传递用户姓名获取热门新闻列表。主要的逻辑和获取推荐页相同，区别在于热门新闻信息主要是通过推荐服务(recsys_server)中的get_hot_list()方法来获取到热门新闻推荐列表



##### 3.4 新闻详情页

server.py 中的 `news_detail()`函数

上面就是详情页的后端逻辑，通过用户名字从mysql中获取用户id信息。防止用户id或者 page id出现空值的情况，需要进行判断。紧接着通过recsys_server服务的get_news_detail()方法，根据新闻的id进行获取内容。

如果用户对该新闻之前点击过喜欢或收藏，再次点击该新闻应该在喜欢或收藏按钮应该是点亮状态，因此还需要根据mysql中再次查询用户与该新闻是否存在记录，并将结果返回给前端，将其进行点亮展示。这里采用两个字段likes和collections，通过True，False来判断用户对该文章之前是否点击过喜欢或收藏。



##### 3.5 用户的行为

在该系统中，用户在看新闻时主要会留下三种用户行为：一是**阅读**，即用户在点击一篇新闻的详细页时，用户产生的行为；二是**喜欢**，在新闻详情页下面会存在喜欢按钮，用户可以通过点击按钮触发系统记录该行为；三是**收藏**，和喜欢行为同理，需要通过用户主动的方式来触发

因此在用户点进一篇新闻的详情页时候，前端会发送一个请求，并给后端传递一个json格式数据：

查看F12调试网络部分，前端会发送一个请求给后端，传递一个json格式数据

点击收藏前端发送的请求

![image-20211221222127887](DataWhale —— 推荐系统学习三(前后端交互).assets/image-20211221222127887.png)

```
{"user_name":"laixiangwei",
"news_id":"fc327a94-b531-47fb-97be-94428f17cde4",
"action_time":1640096449053,
"action_type":"collections:true"}
```

后端处理：

server.py 中的 `actions()`函数

通过前端的传递的数据，后端对应的接口可以通过传递的参数对用户行为进行记录。记录写入`mysql`日志表

**用户行为记录：**

在前端传递过来的数据中存在一个字段 "action_type":"like:ture" 或 "action_type":"like:false"（收藏行为类似），对于action_type参数，其值会是一个组合字符串，冒号前面表示用户的具体行为，冒号后面表示用户当前的行为是点击喜欢还是取消喜欢（例如用户误触导致，用户再次点击则会取消）。

通过**true**和**false**我们不仅可以知道当前用户是点击还是取消，其实还可以知道在数据库中是否存在该用户对该新闻的行为记录。原因是当传递来的是**false**时，表明**like**的状态是从**true**变为**false**，因此数据库中肯定会存在该记录，如果是**true**，表明like的状态是从**false**变为**true**，表明此时数据库中不存在该用户对该新闻的行为记录。通过这样的方式，我们可以比较简单的对数据库进行操作，记录用户的行为。

**用户行为落日志：**

在企业中，任何系统都会有日志的存在，其中最主要的作用是，日志相当于一个监控器，**可以随时监测系统是否出现故障**，通过日志可以及时定位系统中可能存在的问题。但是我们说的日志还有所区别，**我们这里所说的日志主要是记录的一些线上信息，通过日志的方式进行记录**，类似于我们这个系统，用户线上存在的行为，对于我们来说是十分具有意义的，我们需要通过分析这样的用户行为来更好的了解用户兴趣，从而进行更加个性化的推荐。因此我们可以借助日志的方式来记录有意义的用户数据，通过日志数据去分析数据，构建模型，这对于一个算法工程师来说是十分重要的内容。

- 通过日志数据，可以帮助我们更新用户画像中的一些动态特征。
- 在后面构建模型时，我们也能获取到用户的一些点击率，收藏率的建模，为后面的工作提供数据基础。

**新闻动态数据更新**

主要是通过推荐服务里面的 update_news_dynamic_info()方法进行更新

上述代码主要是新闻动态特征更新的部分，主要是获取redis中的信息，根据前端传递过来的行为来更新对用新闻属性的值。更改完之后，从新将新的结果从新存储到redis中。