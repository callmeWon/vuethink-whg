VueThink-whg
===============

## 项目介绍
这是一套基于Vue全家桶（Vue2.x + Vue-router2.x + Vuex）+ Thinkphp的后台管理系统。
脚手架构建也可以通过vue官方的vue-cli脚手架工具构建
实现了一般后台所需要的功能模块

基础支持来自于vuethink开发框架：https://github.com/honraytech/VueThink
VueThink是基于MIT协议的开源框架，它完全免费。你可以免费下载VueThink，用来搭建自己的或者团体的软件。

## 主要适用技术栈
* 后端框架：ThinkPHP 5.0.x/ThinkPHP 5.1.x
* 前端MVVM框架：Vue.JS 2.x
* 开发工作流：Webpack 1.x
* 路由：Vue-Router 2.x
* 数据交互：Axios
* 代码风格检测：Eslint
* UI框架：Element-UI 2.4.11
* JS函数库：Lodash

> VueThink的运行环境要求PHP5.6以上。

详细开发文档参考 [ThinkPHP5完全开发手册](http://www.kancloud.cn/manual/thinkphp5)

## 系统功能

* 登录、退出登录
* 修改密码、记住密码
* 系统参数
* 权限节点
* 岗位管理
* 部门管理
* 用户组管理
* 用户管理
* 完善中......

### 开发依赖

* vue <https://vuefe.cn/v2/guide/>
* element-ui@2.4.11  <http://element-cn.eleme.io/#/zh-CN/component/installation>
* axios  <https://github.com/mzabriskie/axios>
* fontawesome <http://fontawesome.io/icons/>
* js-cookie  <https://github.com/js-cookie/js-cookie>
* lockr  <https://github.com/tsironis/lockr>
* lodash  <http://lodashjs.com/docs/>
* moment  <http://momentjs.cn/>
(引入第三方图标（阿里）：https://www.iconfont.cn/collections/detail?spm=a313x.7781069.1998910419.d9df05512&cid=12507)


## 数据交互

数据交互通过axios以及RESTful架构来实现

用户校验通过登录返回的auth_key放在header

值得注意的一点是：跨域的情况下，会有预请求OPTION的情况

附上接口文档：<http://api.vuethink.com>

## Server搭建
服务端使用的框架为thinkphp5.搭建前请确保拥有lamp/lnmp/wamp环境。

集成环境XAMPP：<https://www.apachefriends.org/index.html>

把项目放入WEB运行环境，如：XAMPP\XAMPP\htdocs下，并使用80端口。

导入服务端根文件夹数据库文件install.sql，(数据库内用户表账号root,数据库名vuethink，密码123456)并修改config/database.php配置文件。

* PHP >= 5.6.0
* PDO PHP Extension
* MBstring PHP Extension
* CURL PHP Extension

服务端开发手册请参考：<http://www.kancloud.cn/manual/thinkphp5/118003>

当访问 <http://localhost>, 出现“vuethink接口”即代表后端接口搭建成功。

## 服务端重写配置
```
请参考[ThinPHP重写](https://www.kancloud.cn/manual/thinkphp5_1/353955)
```



### 环境搭建
```
参考h5学习笔记：vuethink 配置  ：  https://blog.csdn.net/hero82748274/article/details/76100938
```
