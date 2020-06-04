# 海云前端
## 安装Git,并克隆项目至本地
 
 [Git官网下载对应版本](https://git-scm.com/download/)

## Node Js 安装

[Node官网下载对应版本](https://nodejs.org/en/download/)
```
安装完成后请自行检查 node -v 检查版本信息
```
## 前端代码编辑器建议使用 VSCode

[VSCode官网下载对应版本](https://code.visualstudio.com/download/)

### 插件安装

```
 VSCode代码格式化建议使用 vue-format
```
![image](https://github.com/hyrenserv/vcol/blob/master/image/vue-format.jpg)
## 安装Vue-cli脚手架

```
全局安装方式 : npm install -g @vue/cli 如果安装过慢可使用 淘宝镜像 cnpm install -g @vue/cli
```

## 搭建项目
### 项目结构
![image](https://github.com/hyrenserv/vcol/blob/master/image/project-desc.jpg)
 - public 公共模块
   - favicon.ico 项目浏览器访问时的图标
   - index.html 项目编译完成后的首页
 - src 项目的源文件目录
   - assets 项目所需要的公共文件. 如 : css, image等
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
      *  }
      *
      */
     export default new Router({
       routes: [{path: '/', name: 'login', component: () => import('@/hrds/login/login.vue')}]
     })
     ```
### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
