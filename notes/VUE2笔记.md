# VUE2笔记

##  一、项目初始化

### 1.1 项目启动

1. 在terminal输入`npm init -y`
2. 在terminal输入`npm install vue`

之后，我们就可以开始写代码了

### 1.2 hello world

```html
<body>
    <div id="app">
        <h1>{{name}}，非常帅</h1>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el: "#app",
            data: {
                name: "odas"
            }
        });

    </script>
</body>
```

上面一段代码中，我们在第二个script里定义了一个Vue对象，其中，**el**代表元素，我们在上面定义了一个id为**app**的div，在这里我们告诉el我们要操作这个div。在div中，我们用双大括号将name包了起来，这样就可以将它与**data**中的对应变量关联起来了

### 1.3 关于vue的双向绑定

我们先来讲讲vue的结构中的**Model（模型层）**，模型层就是进行参数的定义，数据的操作之类的。然后我们再来讲讲**View（视图层）**，视图层就是用户见到的页面，所谓的**双向绑定**，就是我们在模型层进行的一些操作，比如说参数的值发生改变，那么这也会作用到视图层，页面上的对应值也会发生改变，而反过来，视图层的数据值变了，模型层的参数值也会跟着改变

### 1.4 插件安装

如果想在浏览器上实现vue可视化，可以去谷歌应用商店搜索**vue js devtools**并安装

## 二、指令

### 2.1 v-text, v-html

> 插值闪烁：{{}}这种叫做插值表达式，但是当我们使用插值表达式之后，如果网速过慢，网页里{{}}里的内容还没有加载出来具体值，就会显示变量名，比如{{msg}}，这就叫做插值闪烁。这样其实挺不美观，还会暴露你的命名

- 先说说插值表达式，插值表达式里可以是任意有返回值的式子，比如三元表达式，在vue里定义的函数之类的都可以

- v-text表示内容是纯text，而v-html则表示以html语言的方式读取文本，所以v-text和v-html在读<h1>hello</h1>这样的文本是不一样的

### 2.2 v-bind

v-bind会和html标签的属性绑定，是**单向绑定**，简写为:

### 2.3 v-model

v-model用来和表单项以及自定义组件绑定，比如用户在网页填写的数据，一般就用v-model绑定，是**双向绑定**。

> v-bind和v-model的区别方法是，后面加不加冒号，一般是 v-bind:标签属性，v-mode=给表单起的名字，v-model后面是不能加冒号的

### 2.4 v-on

v-on用来绑定事件，`v-on:`可以简写为`@`

#### 2.4.1 事件修饰符

事件修饰符就是用来操作事件触发的机制的，比如我们点子元素（如：一个div中的div）但是不想父元素也被事件影响到

v-on的修饰符通过`.`来调用即可

- .stop：阻止事件冒泡到父元素

- prevent：阻止默认事件发生

  ```html
  <div @click="showAlert">
      <!-- 比如这里我们点击了div会先跳出一个alert，但是如果我们用.prevent的话，就会阻止页面跳转，我们甚至还可以@click.prevent="showAlert来即阻止它的跳转，又在阻止之后跳出一个alert" -->
  	<a href="https://www.google.com" @click.prevent>去谷歌</a>
  </div>
  ```

- capture：使用事件捕获模式

- self：只有元素自身触发事件才执行（冒泡或捕获的都不执行）

- once：只执行一次

#### 2.4.2 按键修饰符

通过按键来操作事件，比如按一下`↑`键，数字就加2，按键修饰符语法为：`v-on:keyup.XX`

一般常用按键都有别名：

- .enter
- .tab
- .delete
- .space
- .up
- .down
- .left
- .right

还可以按键组合，只要`.enter.up`这样点下去就可以了，还有一个比较特殊的，ctrl+左键，这个时候click在先，这么写`@click.ctrl`

### 2.5 v-for

v-for用来做遍历循环，建议遍历的时候都加上:key来区分不同数据，提高vue渲染效率

### 2.6 v-if和v-show

v-if和v-show都是满足条件后再给予显示，不过v-show更效率，v-show的不显示是隐藏，而v-if的不显示是删除（这会直接修改DOM）

### 2.7 v-else和v-else-if

类似于java的if...else if...else...，if条件满足了，其他的if else就不执行

## 三、计算属性和侦听器

### 3.1 计算属性

**计算属性（==computed==）可以帮我们做一些实时计算，比如，客户在购物车里增加商品数目，我们在下面实时显示总价格的变化**

所有需要动态计算的属性都可以写在computed里，在computed里的值，只要有任何一个发生变化都会触发重新计算

```vue
<script>
    new Vue({
        el: "#app",
        data: {
            xyjPrice: 99.98,
            shzPrice: 98.00,
            xyjNum: 1,
            shzNum: 1
        },
        // 所有需要动态计算的属性都可以写在computed里，在computed里的值，只要有任何一个发生变化
        // 都会触发重新计算
        computed: {
            totalPrice() {
                return this.xyjPrice * this.xyjNum + this.shzPrice * this.shzNum;
            }
        }
	})
</script>
```

### 3.2 监听属性

**通过==watch==可以监控某一个属性**

具体可以看代码

```vue
<script>
    new Vue({
        el: "#app",
        data: {
            xyjPrice: 99.98,
            shzPrice: 98.00,
            xyjNum: 1,
            shzNum: 1,
            msg: ""
        },     
        watch: {
            // 想要监控哪个属性就写在这里，固定套路，比如这里我们写上xyjNum，这个xyjNum是一个方法(function)
            // 这个方法传入两个参数，前面的参数(newVal)是新值，后面的参数(oldVal)是旧值
            // 一旦xyjNum的值发生变化，function会自动触发
            // 这里是函数定义xyjNum: function (newVal, oldVal)的简写(es6新特性)
            xyjNum(newVal, oldVal) {
                alert("oldVal: " + oldVal + " ==> newVal: " + newVal);
                if (newVal > 3) {
                    this.msg = "库存超出限制";     
                    // 这里限制一下让xyjNum不能增加，但是又不能限制到3，因为如果xyjNum变成3
                    // 又会触发else条件，导致msg变为空，会无法显示"库存超出限制"
                    this.xyjNum = 4;                 
                } else {
                    this.msg = "";
                }
            }
        }
    })
</script>
```

### 3.3 过滤器

过滤器常用来处理文本格式化的操作。过滤器可以用在两个地方:双花括号插值和v-bind表达式，这里我们演示双括号插值表达式的过滤器使用

```html
<body>
    <!-- 过滤器常用来处理文本格式化的操作。过滤器可以用在两个地方:双花括号插值和v-bind表达式 -->
    <div id="app">
        <ul>
            <li v-for="user in userList">
                <!-- 不用过滤器的话这里用了个三元运算符 -->
                <!-- 但是如果要做更复杂的处理，可以使用过滤器，过滤器可以使用管道符'|'触发 -->
                <!-- user.gender | genderFilter就是把user.gender的值作为参数传给genderFilter函数来处理，然后显示函数的返回值 -->
                {{user.id}} ==> {{user.name}} => {{user.gender == 1 ? "男" : "女"}} =>
                {{user.gender | genderFilter}} => {{user.gender | gFilter}}
            </li>
        </ul>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        Vue.filter("gFilter", function (val) {
            if (val == 1) {
                return "MALE";
            } else {
                return "FEMALE";
            }
        });

        let vm = new Vue({
            el: "#app",
            data: {
                userList: [
                    { id: 1, name: 'alice', gender: 1 },
                    { id: 2, name: 'peter', gender: 0 }
                ]
            },
            filters: {
                // filters定义局部过滤器，只可以在当前vue实例中使用，其他用不了，但是我们可以定义全局过滤器
                genderFilter(val) {
                    if (val == 1) {
                        return "m";
                    } else {
                        return "female";
                    }
                }
            }
        });
    </script>
</body>
```



## 四、组件化

大型应用开发的时候，页面可以划分成很多部分，有些部分可以重复使用，所以只要我们把页面的不同的部分拆分成独立的组件，就可以到处使用，避免重复开发啦。在vue里，所有vue实例都是组件。~~不过现在其实更多的使用脚手架了，这个熟悉一下就好~~

示例代码如下：

```html
<body>
    <div id="app">
        <button v-on:click="count++">我被点击了 {{count}} 次</button>

        <!-- 想要用组件只要写一个标签即可 -->
        <counter></counter>
        <counter></counter>

        <button-counter></button-counter>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        // 1. 全局声明一个组件
        Vue.component("counter", {
            // 定义一个html模板，组件会和这个模板绑定，然后这个模板就可以到处被覆用啦
            template: `<button v-on:click="count++">我被点击了 {{count}} 次</button>`,
            // data必须写成一个方法，其实和创建一个vue实例挺像的
            // 可以理解为new了一个vue组件(只是这里叫data)，然后它的el是绑定到template去的            
            data() {
                // 而且这里要再套一层return，因为data是一个函数的形式
                return {
                    // count的初始值是1
                    count: 1
                }
            }
        });

        // 2. 局部声明一个组件，需要在特定的vue实例里通过components声明
        const buttonCounter = {
            template: `<button v-on:click="count++">(局部)我被点击了 {{count}} 次</button>`,
            data() {
                return {
                    count: 1
                }
            }
        };

        new Vue({
            el: "#app",
            data: {
                count: 1
            },
            components: {
                // 冒号左边是组件名，可以随便起，注意这里有特殊符号所以我们用单引号，如果组件名和函数名一样，可以只写函数名字
                'button-counter': buttonCounter
            }
        });
    </script>
</body>
```



## 五、生命周期和钩子(hook)函数

每个Vue实例在被创建时都要经过一系列的初始化过程：创建实例，装载模板，渲染模板等等。Vue为生命周期中的每个状态都设置了钩子函数(监听函数)



## 六、Vue模块化开发

==注意windows控制台右键属性需要把快速编辑取消掉，否则点了正在运行的控制台就会暂停==

### 1. npm install webpack -g

全局安装webpack

### 2. npm install -g @vue/cli-init

Vue的控制台，全局安装vue脚手架，如果还出现找不到vue命令，则再在控制台输入`npm install vue-cli -g`

### 3. 初始化vue项目

vue init webpack appname: vue脚手架使用webpack模板初始化一个appname项目，跳出来的项目名和项目描述可以写也可以不写，不写就按回车即可，vue-build那里选则第一个recommended for most users，除了Install vue-router?其他都选no

然后根据提示`npm run dev`就可以启动项目，输入localhost:8080即可进入首页

- 如果是新版，则`npm run server`即可启动项目

### 4、启动vue项目

项目的package.json中有scripts，代表我们能运行的命令

> 这里补一下App.vue的代码

```vue
<template>
  <!-- 下面这段只是用来熟悉router-link的 -->
  <div id="app">    
    <img src="./assets/logo.png">
    <!-- 这个是vue版的超链接，可以跳转到在router里定义的页面 -->
    <router-link to="/hello">去hello</router-link>
    <router-link to="/">去首页</router-link>
    <!-- 路由视图 -->
    <router-view/>
  </div>

</template>

<script>
export default {
  name: 'App'
}
</script>
```





## 七、整合element ui

==vue3的话就使用element plus==

### 1. 使用npm安装

`npm i element-ui -S`

### 2. 引入 Element组件

在main.js中写入一下内容

```javascript
import Vue from 'vue';
import ElementUI from 'element-ui';		// 重要的行，导入组件
import 'element-ui/lib/theme-chalk/index.css';	// 重要的行，导入组件
import App from './App.vue';

// 重要的行，通过use表示正式使用组件
Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

### 3. 可以挑一个Element组件测试一下

### 4. 在vscode里创建vue模板

文件 ➡ 首选项 ➡ 用户代码片段 ➡ 点击新建全局代码片段 ➡ 取名 vue ➡ 确定，这样之后直接在文件里输入vue点回车就会跳出模板

```json
{
    "Print to console": {
        "prefix": "vue",
        "body": [
            "<!-- $1 -->",
            "<template>",
            "<div class='$2'>$5</div>",
            "</template>",
            "",
            "<script>",
            "//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）",
            "//例如：import 《组件名称》 from '《组件路径》';",
            "",
            "export default {",
            "//import引入的组件需要注入到对象中才能使用",
            "components: {},",
            "data() {",
            "//这里存放数据",
            "return {",
            "",
            "};",
            "},",
            "//监听属性 类似于data概念",
            "computed: {},",
            "//监控data中的数据变化",
            "watch: {},",
            "//方法集合",
            "methods: {",
            "",
            "},",
            "//生命周期 - 创建完成（可以访问当前this实例）",
            "created() {",
            "",
            "},",
            "//生命周期 - 挂载完成（可以访问DOM元素）",
            "mounted() {",
            "",
            "},",
            "beforeCreate() {}, //生命周期 - 创建之前",
            "beforeMount() {}, //生命周期 - 挂载之前",
            "beforeUpdate() {}, //生命周期 - 更新之前",
            "updated() {}, //生命周期 - 更新之后",
            "beforeDestroy() {}, //生命周期 - 销毁之前",
            "destroyed() {}, //生命周期 - 销毁完成",
            "activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发",
            "}",
            "</script>",
            "<style lang='scss' scoped>",
            "//@import url($3); 引入公共css类",
            "$4",
            "</style>"
        ],
        "description": "生成vue模板"
    },
    "http-get请求": {
	"prefix": "httpget",
	"body": [
		"this.\\$http({",
		"url: this.\\$http.adornUrl(''),",
		"method: 'get',",
		"params: this.\\$http.adornParams({})",
		"}).then(({ data }) => {",
		"})"
	],
	"description": "httpGET请求"
    },
    "http-post请求": {
	"prefix": "httppost",
	"body": [
		"this.\\$http({",
		"url: this.\\$http.adornUrl(''),",
		"method: 'post',",
		"data: this.\\$http.adornData(data, false)",
		"}).then(({ data }) => { });" 
	],
	"description": "httpPOST请求"
    }
}
```

### 5. 









