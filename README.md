# 海云前端
## 安装Git,并克隆项目至本地
 
 [Git官网下载对应版本](https://git-scm.com/download/)

## Node Js 安装

[Node官网下载对应版本](https://nodejs.org/en/download/), 完成后请自行检查npm指令
```
安装完成后请自行检查 npm -v 检查版本信息
```
## 前端代码编辑器建议使用 VSCode

[VSCode官网下载对应版本](https://code.visualstudio.com/download/)

## 插件安装
### 打开VSCode,并进行如下安装操作
```
 VSCode代码格式化建议使用 vue-format
```
![image](https://github.com/hyrenserv/vcol/blob/master/image/vue-format.jpg)
### 安装Vue-cli脚手架
 - vue-cli主要用于创建用户界面
```
全局安装方式 : npm install -g @vue/cli, 如果安装过慢可使用淘宝镜像 cnpm install -g @vue/cli
```
 - 前端UI框架使用 element-ui
 ```
  安装 npm install element-ui -S
 ```
## 搭建项目
### 项目结构
![image](https://github.com/hyrenserv/vcol/blob/master/image/project-desc.jpg)
 - public 公共模块
   - favicon.ico 项目浏览器访问时的图标
   - index.html 首页入口文件
 - src 项目的源文件目录
   - assets 项目所需要的公共文件. 如: css, image等
   - hrds 所有项目的根目录
   - router 整体项目路由的配置
     - 路由配置说明(index.js)
     ```js
     import Vue from 'vue'
     import Router from 'vue-router'
     /**
      *
      * 路由配置信息
      *  routes : [{这里的第一层表示为普通的路由地址,children : [{这个里面的是二级路由配置}]
      * 路由配置如 :
      *  {
      *      @param path: "/home"   表示的是路由匹配路径,
      *      @param name: 'home' 路由的名称
      *      @param component: () => import('@/hrds/login/index') 后面为路由地址的具体页面
      *      @param children: 路由下的子路由
      *  }
      *
      */
     export default new Router({
       routes: [{path: '/', name: 'login', component: () => import('@/hrds/login/login.vue'),children: [
                {path: '/syspara', name: 'syspara', component: () => import('@/hrds/a/syspara/index.vue')}}]
     })
     ```
   - store 存储公共的数据, 如:保存用户登陆信息,清空用户登陆信息等
   - util 存放公共js的地方,如: 验证,代码项的获取及请求后台的js文件
   - App.vue 是我们的主组件，所有页面都是在App.vue下进行切换的。其实你也可以理解为所有的路由也是App.vue的子组件。所以我将router标示为          App.vue的子组件。 
   - main.js 是我们的入口文件,主要作用是初始化vue实例并使用需要的插件.此文件中可以引入公共的js或者第三方的js,并提供全局使用
     - 第三方文件的安装方式 npm install [插件] -S 或者 cnpm install [插件] -S, -S 表示直接将插件信息保存至package.json和package-lock.json文件中
        ![image](https://github.com/hyrenserv/vcol/blob/master/image/install-components.jpg)
	![image](https://github.com/hyrenserv/vcol/blob/master/image/install-components2.jpg)
     - 在main.js的引用方式如: 
     ```js
       // 引入echarts
       import echarts from 'echarts'
       Vue.prototype.$echarts = echarts   
     ```
     - 引入前端UI,请先全局安装element-ui( npm install element-ui -S )
     ```js
     	//在main.js引入UI框架
     	import ElementUI from 'element-ui';
	// 并注册到Vue的全局中
	Vue.use(ElementUI);
     ```
     - 在.vue文件中的使用方式如:
     ```js
       // 基于准备好的dom，初始化echarts实例
       let myChart = this.$echarts.init(document.getElementById('myChart'));
     ```
   - .env.development 开发服务地址的配置
     ![image](https://github.com/hyrenserv/vcol/blob/master/image/serverconf.jpg)
     ```
       #----A工程代理地址
       VUE_APP_HRDS_A_API = 'http://127.0.0.1:8888' 
     ```
   - .env.production 生产服务地址配置 (同开发配置一致)
   
   - babel.config.js 对地址下的文件进行转码
   
   - package-lock.json 的作用就是锁定安装依赖时包的版本，以确保其他人npm install时安装的依赖能夠保持一致
   
   - package.json 打包项目时的配置信息,如: 打包命令,启动命令及每个插件的版本控制等
     ![image](https://github.com/hyrenserv/vcol/blob/master/image/package.jpg)
     
   - vue.config.js 项目启动时的代理配置,主要防止出现请求时的跨域及服务器过多配置繁琐的问题
     ![image](https://github.com/hyrenserv/vcol/blob/master/image/proxyconf.jpg)
   ```js
       module.exports = {
          publicPath: './',
          outputDir:'dist',
          assetsDir:'static',
          productionSourceMap: false,
	         lintOnSave: process.env.NODE_ENV === 'development',
          devServer: {
              open: false,//启动时,是否自动打开浏览器
              https: false,//是否为 https的方式
              overlay: {
                warnings: false,
                errors: true
              },
              proxy: {
            //工程 A 代理配置规则
                '/A': {
                  target: process.env.VUE_APP_HRDS_A_API,	// 目标 API 地址
                  changeOrigin: true,	// 允许websockets跨域
                  ws: true,
                  pathRewrite: { //匹配到 /A时,将/A地址重写为 /A/action/hrds/a/biz
                    '^/A': '/A/action/hrds/a/biz'
                  }
             }
              }
            }
         }
     ```
### 请求后台方式
 - 定义请求方式,假设我定义了个参数请求的方法(syspara.js)
   ![image](https://github.com/hyrenserv/vcol/blob/master/image/rquestjsconf.jpg)
   - 转换请求流程如图:
   ![image](https://github.com/hyrenserv/vcol/blob/master/image/relyconf.jpg)
   syspara.js, 定义方式如:
   ```js
     //引入封装后请求后台的公共js
     import request from '@/utils/request'
     /**
      * 获取参数列表信息
      * @returns {JSON} : 返回JSON数据
      */
     export function getSysPara() {
       return request({
         url: '/A/syspara/getSysPara',
       })
     }
   ```
 -  请求后台(syspara.vue)
   syspara.vue,内容如下:
   ![image](https://github.com/hyrenserv/vcol/blob/master/image/requestservice.jpg)
   
### 数据展现方式
 -
