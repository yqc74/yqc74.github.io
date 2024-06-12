---
layout: page
permalink: /blogs/JavaWeb/index.html
title: JavaWeb
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
