---
layout: page
permalink: /blogs/Vue/index.html
title: Vue
---

# 创建Vue3项目

**使用npm或yarn包管理工具都可以搭配Vite手动创建项目**

## 手动创建Vue项目

yarn create vite![image-20240611104913860](C:\Users\李jiyang\AppData\Roaming\Typora\typora-user-images\image-20240611104913860.png)

![image-20240611104931446](C:\Users\李jiyang\AppData\Roaming\Typora\typora-user-images\image-20240611104931446.png)

最后进入创建的项目里面输入命令- yarn



## **通过模板自动创建项目的命令**

yarn create vite hello-vite --template vue

最后进入创建的项目里面输入命令- yarn



## Vue 3项目的目录结构

- **.vscode**：存放VS Code编辑器的相关配置。
- **node_modules**：存放项目的各种依赖和安装的插件。
- **public**：存放不可编译的静态资源文件，当进行项目构建时，该目录下的文件会被复制到dist目录，该目录下的文件需要使用绝对路径访问。
- **src**：源代码目录，保存开发人员编写的项目源代码。
- **src\assets**：存放可编译的静态资源文件，例如图片、样式文件等。该目录下的文件需要使用相对路径访问。
- **src\components**：存放单文件组件，即.vue文件。
- **src\components\HelloWorld.vue**：一个名称为HelloWorld的单文件组件。
- **src\App.vue**：项目的根组件。
- **src\main.js**：项目的入口文件，用于创建Vue应用实例。
- **src\style.css**：项目的全局样式表文件。
- **.gitignore**：向Git仓库上传代码时需要忽略的文件列表。
- **index.html**：默认的主渲染页面文件，同时也是页面的入口文件。
- **package.json**：包配置文件。
- **README.md**：项目使用说明文件。
- **vite.config.js**：存放Vite的相关配置。
- **yarn.lock**：存储每一个依赖项的安装版本，在使用yarn安装、升级、卸载依赖时，会自动更新yarn.lock文件。

==注==：运行vue3项目时，记得 yarn -dev 启动服务



# Vue.js开发基础

# 单文件组件

**定义方式**

```html
<template>  
    <!-- 此处编写组件的结构 -->
</template>
<script>
    /* 此处编写组件的逻辑 */
</script>
<style>
    /* 此处编写组件的样式 */
</style>
```

==**ps**==：项目创建完成后，执行如下命令进入项目目录，启动项目。也可以在vscode终端运行，**如遇权限问题**，可1.搜索powershell，右键以管理员身份运行;2.输入: set-ExecutionPolicy RemoteSigned ;3.输入y回车键确认即可;



## 数据绑定

### 定义数据

```html
<script>
export default {
  setup() {
    return {
      数据名: 数据值,
      ……
    }
  }
}
</script>

<!-- setup语法糖，简化上述语法-->
<script setup>
	const 数据名 = 数据值
</script>

<!-- 输出数据-->
<template>
   {{数据名}} 
</template>
```

### 响应式数据

#### ref()函数

```html
<!-- 等待两秒刷新响应数据-->
<template>{{ message }}</template>
<script setup>
    import { ref } from 'vue'
    const message = ref('会当凌绝顶，一览众山小')
    setTimeout(() => {
      message.value = '锲而不舍，金石可镂'
    }, 2000)
</script>
```

将数据定义成响应式数据

#### reactive()函数

用于创建一个响应式对象或数组，将普通的对象或数组作为参数传给该函数即可。

```html
<template>
    {{ obj.message }}
</template>
<script setup>
    import { reactive } from 'vue'
    const obj = reactive({ message: '不畏浮云遮望眼，自缘身在最高层' })
    setTimeout(() => {
      obj.message = '欲穷千里目，更上一层楼'
    }, 2000)
</script>
```

#### toRef()函数

用于将响应式对象中的单个属性转换为响应式数据。

```html
<template>
  <div>message的值：{{ message }}</div>
  <div>obj.message的值：{{ obj.message }}</div>
</template>
<script setup>
    import { reactive, toRef } from 'vue'
    const obj = reactive({ message: '黑发不知勤学早，白首方悔读书迟' })
    const message = toRef(obj, 'message')
    setTimeout(() => {
      message.value = '少壮不努力，老大徒伤悲'
    }, 2000)
</script>
```

#### toRefs()函数

用于将响应式对象中的所有属性转换为响应式数据。

```html
<template>
  <div>message的值：{{ message }}</div>
  <div>obj.message的值：{{ obj.message }}</div>
</template>
<script setup>
    import { reactive, toRefs } from 'vue'
    const obj = reactive({ message: '盛年不重来，一日难再晨' })
    let { message } = toRefs(obj)
    setTimeout(() => {
      message.value = '及时当勉励，岁月不待人'
    }, 2000)
</script>
```



## Vue指令

| **指令**  | **作用**                                            |
| --------- | --------------------------------------------------- |
| v-bind    | 为HTML标签绑定属性值，如设置  href , css样式等      |
| v-model   | 在表单元素上创建双向数据绑定                        |
| v-on      | 为HTML标签绑定事件                                  |
| v-if      | 条件性的渲染某元素，判定为true时渲染,否则不渲染     |
| v-else    |                                                     |
| v-else-if |                                                     |
| v-show    | 根据条件展示某元素，区别在于切换的是display属性的值 |
| v-for     | 列表渲染，遍历容器的元素或者对象的属性              |

- v-bind:  为HTML标签绑定**属性值**，如设置  href , css样式等。当vue对象中的数据模型发生变化时，标签的属性值会随之发生变化。

- v-model： 在表单元素上创建**双向数据绑定**。什么是双向？

  -  vue对象的data属性中的数据变化，视图展示会一起变化
  -  视图数据发生变化，vue对象的data属性中的数据也会随着变化。
  -  ==**v-model内部会为不同的元素绑定不同的属性和事件**==
     -  textarea元素和text类型的input元素会绑定value属性和input事件。
     -  checkbox类型的input元素和radio类型的input元素会绑定checked属性和change事件。
     -  select元素会绑定value属性和change事件。

  - **双向绑定的作用：可以获取表单的数据的值，然后提交给服务器**

    | 修饰符  | 作用                                    |
    | :------ | --------------------------------------- |
    | .number | 自动将用户输入的值转换为数字类型        |
    | .trim   | 自动过滤用户输入的首尾空白字符          |
    | .lazy   | 在change事件而非input事件触发时更新数据 |

- v-on: 用来给html标签绑定**事件**的。**需要注意的是如下2点**：

  - v-on语法给标签的事件绑定的函数，必须是vue对象中声明的函数


  - v-on语法绑定事件时，事件名相比较js中的事件名，没有on

- v-if：根据布尔值切换元素的显示或隐藏状态，本质是通过操作DOM元素来切换显示状态。

- v-show：通过为元素添加或移除display: none样式来实现元素的显示或隐藏。
  - **当需要频繁切换某个元素的显示或隐藏时，使用v-show会更节省性能开销；而当只需要切换一次显示或隐藏时，使用v-if更合理。**

- v-for：
  - 根据数组渲染列表：<标签名 v-for="(item, index) in arr"></标签名>
  - 根据对象渲染列表：<标签名 v-for="(item, name, index) in obj"></标签名>
  - 根据数字渲染列表：<标签名 v-for="(item, index) in num"></标签名>
  - 根据字符串渲染列表：<标签名 v-for="(item, index) in str"></标签名>



# 组件基础

## 动态组件

- 语法 ： <component :is="要渲染的组件"></component>



## 组件缓存

- 组件缓存可以使组件创建一次后，不会被销毁。在Vue中可以通过KeepAlive组件来实现组件缓存。

- 问题：当一个组件被销毁后又重新创建时，组件无法保持销毁前的状态。

- 语法

  ```html
  <KeepAlive>
  	<!-- 需要缓存的组件 -->
  </KeepAlive>
  ```

  

### 组件缓存相关的生命周期函数

```html
// onActivated()生命周期函数被激活
onActivated(() => { })
// onDeactivated()生命周期函数被缓存
onDeactivated(() => { })

<script setup>
import { ref, onMounted, onUnmounted, onActivated, onDeactivated } from 'vue'
onActivated(() => {
  console.log('MyLeft组件被激活了')
})
onDeactivated(() => {
  console.log('MyLeft组件被缓存了')
})
</script>
```



### KeepAlive组件的常用属性

| 属性    | 类别               | 说明                       |
| ------- | ------------------ | -------------------------- |
| include | 字符串或正则表达式 | 只有名称匹配的组件会被缓存 |
| exclude | 字符串或正则表达式 | 名称匹配的组件不会被缓存   |
| max     | 数字               | 最多可以缓存组件实例个数   |

==**注意**==：在使用KeepAlive组件对名称匹配的组件进行缓存时，它会根据组件的name选项进行匹配。如果没有使用setup语法糖，必须手动声明name选项；如果使用了setup语法糖，Vue会根据文件名自动生成name选项，无须手动声明name选项。例如，在MyLeft.vue文件中使用setup语法糖时，自动生成的组件名为MyLeft。

- 非语法糖定义name选项的示例代码

  ```html
  <script>
      export default {
        name: 'MyComponent'
      }
  </script>
  ```

  

# 插槽

- 插槽是指开发者在封装组件时不确定的、希望由组件的使用者指定的部分。

- 语法：

  ```html
  <!-- 子组件定义插槽-->
  <template>
    <button>
      <slot>
       	<!-- 内容--> 
       </slot>
    </button>
  </template>
  
  <!-- 父组件使用插槽-->
  <template>
     <MyButton>
        <span style="color: yellow;">按钮</span>	//按钮
     </MyButton>
  </template>
  <!-- 父组件使用插槽2-->
  <template>
    父组件-----{{ message }}
    <hr>
    <SlotSubComponent>
      <p>{{ message }}</p>
    </SlotSubComponent>
  </template>
  
  ```
  
  ==**注**==：插槽内容是在父组件模板中定义的，所以在插槽内容中可以访问到父组件的数据，父组件会顶替掉子组件里面的内容

## 具名插槽

- 问题：在Vue中当需要定义多个插槽时，可以通过具名插槽来区分不同的插槽。具名插槽是给每一个插槽定义一个名称，这样就可以在对应名称的插槽中提供对应的数据了。

- 子组件中语法： <slot name="插槽名称"></slot>

- 父组件中语法：

  ```html
  <组件名>
    <template v-slot:插槽名称>
    	<!-- 插入的内容-->
    </template>
  </组件名>
  ```


例子：

```html
<!-- 父组件-->
<template>
  <SubScopeSlot>
    <template #default="scope">
      <p>{{ scope }}</p>
    </template>
    <template v-slot:header="scope">
      <p>{{ scope }}</p>
      <p>{{ scope.message }}</p>
    </template>
    <template #content="{ user }">
      <p>{{ user.name }}</p>
      <p>{{ user.age }}</p>
    </template>
  </SubScopeSlot>
</template>


<!-- 子组件-->
<template>
  <slot message="Hello 默认插槽" message2="Hello 默认插槽"></slot>
  <hr>
  <slot message="Hello Vue.js" name="header"></slot>
  <hr>
  <slot :user="user" name="content"></slot>
</template>

```



# 自定义指令

## 私有自定义指令

| 函数名          | 说明                                                 |
| --------------- | ---------------------------------------------------- |
| created()       | 在绑定元素的属性前调用                               |
| beforeMount()   | 在绑定元素被挂载前调用                               |
| mounted()       | 在绑定元素的父组件及自身的所有子节点都挂载完成后调用 |
| beforeUpdate()  | 在绑定元素的父组件更新前调用                         |
| updated()       | 在绑定元素的父组件及自身的所有子节点都更新后调用     |
| beforeUnmount() | 在绑定元素的父组件卸载前调用                         |
| unmounted()     | 在绑定元素的父组件卸载后调用                         |

| 参数     | 说明                                         |
| -------- | -------------------------------------------- |
| el       | 指令所绑定的元素，可以直接用于操作DOM元素    |
| binding  | 一个对象，包含很多属性，用于接收属性的参数值 |
| vnode    | 代表绑定元素底层的虚拟节点                   |
| prevNode | 之前页面渲染中指令所绑定元素的虚拟节点       |

主要使用mounted()和updata()

### 示例代码

定义的时候：驼峰命名法
使用的时候：v-

```html
<template>
  	<span v-color></span>
</template>
<script setup>
	const vColor = { }
</script>
```



## 全局自定义指令

**演示自定义指令参数的使用方法**

```html
<template>
  <p v-fontSize="fontSize">DirectiveComponent组件</p>
  <button @click=“fontSize = ‘24px’”>更改字号大小</button>
</template>
<script setup>
    import { ref } from 'vue'
    const fontSize = ref('12px')
    const vFontSize = {
      mounted: (el, binding) => { el.style.fontSize = binding.value },
      updated: (el, binding) => { el.style.fontSize = binding.value }
	}

</script>

```

**私有自定义指令简化为函数形式**

```html
<script>
	const vFontSize = (el, binding) => {
      el.style.fontSize = binding.value
	}
</script>
```



# 路由

## 初识路由

### 概念

指路由器从一个接口接收到数据，根据数据的目的地址将数据定向传送到另一个接口的行为和动作

### 后端路由

- 后端路由的整个过程发生在服务器端，开发者需要在服务器端程序中建立一套后端路由规则。当服务器接收到请求后，会通过路由寻找当前请求的URL地址对应的处理程序
- 当服务器接收到浏览器的请求后，会通过app.get()方法根据URL地址中的路径寻找对应的处理程序。

### 前端路由

前端路由的整个过程发生在浏览器端，其特点是当URL地址改变时不需要向服务器发起一个加载新页面的请求，而是在维持当前页面的情况下切换页面中显示的内容。

- ① Hash模式Hash模式的前端路由通过URL中从“#”开始的部分实现不同组件之间的切换，“#”表示Hash符，“#”后面的值称为Hash值，该值将用于进行路由匹配。
- ② HTML5模式HTML5模式的URL地址与后端路由的URL地址的风格一致，可以通过URL地址中的路径进行路由匹配。



## Vue Router

## 路由的基本使用步骤

1. 定义路由组件
   - 在src\components目录下创建Home.vue文件和About.vue文件。

1. 定义路由链接和路由视图

   - 使用<router-view>标签定义路由视图，该标签会被渲染成当前路由对应的组件。

   - 通过<router-link>标签定义路由链接方便在不同组件之间切换。

   - ```html
     <template>
       <div class="app-container">
         <h1>App根组件</h1>
         <router-link to="/home">首页</router-link>
         <router-link to="/about">关于</router-link>
         <hr>
         <router-view></router-view>
       </div>
     </template>
     <style scoped>
         .app-container {
           text-align: center;
           font-size: 16px;
         }
         .app-container a {
           padding: 10px;
           color: #000;
         }
         .app-container a.router-link-active {
           color: #fff;
           background-color: #000;
         }
     </style>
     ```

2. 创建路由模块

   - 在src目录下创建router.js文件作为路由模块，并在该文件中导入路由相关函数。
     import { createRouter, createWebHashHistory } from 'vue-router'

   - 在router.js文件中导入需要被路由控制的Home组件和About组件
     import Home from './components/Home.vue'
     import About from './components/About.vue'

   - 在router.js文件中创建路由实例对象。

     ```HTML
     const router = createRouter({
       history: createWebHashHistory(),
       routes: [
         { path: '/home', component: Home },
         { path: '/about', component: About },
       ]
     })
     
     <!-- 懒加载-->
     <!-- 点击之后加载，防止打开页面加载太多组件而卡顿-->
     const router = createRouter({
       history: createWebHashHistory(),
       routes: [
         { path: '/home', component: () => import('./components/Home.vue') },
         { path: '/about', component: () => import('./components/About.vue') },
       ]
     })
     ```

   - 在router.js文件中导出路由实例对象。
     export default router

3. 导入并挂载路由模块

```html
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router.js'	// 导入路由模块
const app = createApp(App)
app.use(router)		// 挂载路由模块
app.mount('#app')
```

## 路由重定向

### 定义

路由重定向可以使用户在访问一个URL地址时，强制跳转到另一个URL地址，从而展示特定的组件。通过路由匹配规则中的redirect属性可以指定一个新的路由地址，从而实现路由重定向。

## 嵌套路由

### 定义

嵌套路由是指通过路由实现组件的嵌套展示，它主要由页面结构决定。实际项目中的应用界面通常由多层嵌套的组件组合而成，为了使多层嵌套的组件能够通过路由访问，路由也需要具有嵌套关系，也就是在路由里面嵌套它的子路由。

### 使用

在src\router.js文件的路由匹配规则中通过children属性定义子路由匹配规则。

```html
routes: [
  {
    path: '父路由路径', 
    component: 父组件,
    children: [
      { path: '子路由路径1', component: 子组件1 },
      { path: '子路由路径2', component: 子组件2 }
    ]
  }
]
<!-- 在组件中定义子路由链接的语法格式-->
<router-link to="/父路由路径/子路由路径"></router-link>

<!-- 案例-->
<!-- component\About.vue文件中添加子路由链接和子路由视图-->
<template>
  <div class="about-container">
    <h3>About组件</h3>
    <router-link to="/about/tab1">tab1</router-link>
    <router-link to="/about/tab2">tab2</router-link>
    <hr>
    <router-view></router-view>
  </div>
</template>

<!-- 修改src\router.js文件，在routes中导入Tab1组件和Tab2组件，并使用children属性定义子路由匹配规则-->
routes: [
  { path: '/', redirect: '/about' },
  { path: '/home', component: () => import ('./components/Home.vue') },
  { path: '/about', component: () => import('./components/About.vue'),
        children: [
          { path: 'tab1', component: () => import ('./components/pages/Tab1.vue') },
          { path: 'tab2', component: () => import ('./components/pages/Tab2.vue') }
		]
  }]
```

## 动态路由

### 概念

动态路由是一种路径不固定的路由，路径中可变的部分被称为动态路径参数（Dynamic Segment），使用动态路径参数可以提高路由规则的可复用性。在Vue Router的路由路径中，使用“:参数名”的方式可以在路径中定义动态路径参数。

**代码演示**：{ path: '/sub/:id', component: 组件 },
动态路由参数id为参数名

### 获取

- 使用$route.params获取参数值
- 使用props获取参数值。

#### $route.params

- **新建src\components\Movie.vue文件，在该文件中定义3个路由链接和路由视图。**

- ```html
  <template>
    <div class="movie-container">
      <router-link to="/movie/1">电影1</router-link>
      <router-link to="/movie/2">电影2</router-link>
      <router-link to="/movie/3">电影3</router-link>
      <router-view></router-view>
    </div>
  </template>
  <style>
  .movie-container {
      min-height: 150px;
    background-color: #f2f2f2;
  }
  .movie-container a {
    padding: 0 5px;
    font-size: 18px;
    border: 1px solid #ccc;
    border-radius: 5px;
    color: #000;
    margin: 0 5px;
  }
  </style>
  
  ```

  #### props获取参数值
  
  1. 修改src\components\MovieDetails.vue文件，使用props接收路由规则中匹配到的参数
  
     ```html
     <template>
       <p>电影{{ id }}页面</p>
     </template>
     <script setup>
     const props = defineProps({
       id: String
     })
     </script>
     ```
  
  2. 在src\router.js文件中，为“:id”路径的路由开启props传参
  
     ```html
     {
       path: ':id', 
       component: () => import          ('./components/movieDetails.vue'), 
       props: true 
     }
     ```

## 命名路由

### 命名路由的语法格式

{ path: '路由路径', name: '路由名称', component: 组件 }

<router-link>标签中使用命名路由：

- 动态绑定to属性的值为对象。当使用对象作为to属性的值时，to前面要加一个冒号，表示使用v-bind指令进行绑定。在对象中，通过name属性指定要跳转到的路由名称，使用params属性指定跳转时携带的路由参数
  <router-link :to="{ name: 路由名称, params: { 参数名: 参数值 } }"></router-link>

### 使用

在src\components\Home.vue文件中的<router-link>标签中动态绑定to属性的值，指定要跳转到的路由名称，并传递参数。

```html
<template>
  <div class="home-container">
   <h3>Home组件</h3>
   <router-link :to="{ name: 'MovieDetails', params: { id: 3 } }">跳转到MovieDetails组件</router-link>
  </div>
</template>
```

在src\router.js文件中，将“:id”路径的路由名称定义为MovieDetails。

```html
{ path: ':id', name: 'MovieDetails', component: () => import ('./components/movieDetails.vue'), props: true }
```

