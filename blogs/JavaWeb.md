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



toRef()函数

toRefs()函数

