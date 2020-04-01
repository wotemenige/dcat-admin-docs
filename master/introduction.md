# Dcat Admin

`Dcat Admin` 是一个基于[laravel-admin](https://www.laravel-admin.org/)二次开发而成的后台构建工具，只需极少的代码即可构建出一个功能完善且颜值极高的后台系统。

<p align="center">
    <a href="https://github.com/jqhph/dcat-admin/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-7389D8.svg?style=flat" ></a>
    <a href="https://travis-ci.org/jqhph/dcat-admin">
        <img src="https://travis-ci.org/jqhph/dcat-admin.svg?branch=master" alt="Build Status">
    </a>
    <a href="https://styleci.io/repos/182349597">
        <img src="https://github.styleci.io/repos/182349597/shield" alt="StyleCI">
    </a>
    <a href="https://packagist.org/packages/dcat/laravel-admin" ><img src="https://poser.pugx.org/dcat/laravel-admin/v/stable" /></a> 
    <a href="https://packagist.org/packages/dcat/laravel-admin"><img src="https://img.shields.io/packagist/dt/dcat/laravel-admin.svg?color=" /></a> 
    <a><img src="https://img.shields.io/badge/php-7.1+-59a9f8.svg?style=flat" /></a> 
    <a><img src="https://img.shields.io/badge/laravel-5.5+-59a9f8.svg?style=flat" ></a>
</p>

## 技术栈

- [Laravel](https://laravel.com/)
- [Bootstrap4](https://getbootstrap.net/)
- jQuery3等

## 特性

- 用户管理（可拆卸）
- RBAC权限管理（可拆卸），支持无限极权限节点
- 菜单管理（可拆卸）
- 使用PJAX构建无刷新页面，并且支持按需加载静态资源
- 松耦合的页面构建与数据操作设计，可轻松切换数据源
- 插件功能
- 可视化代码生成器，可根据数据表一键生成增删改查页面
- 数据表格构建工具，内置丰富的表格常用功能（如组合表头、数据导出、搜索、快捷创建、批量操作等）
- 树状表格功能构建工具，支持分页和点击加载
- 数据表单构建工具，内置 50+ 种表单类型，支持异步提交
- 分步表单构建工具
- 弹窗表单构建工具
- 数据详情页构建工具
- 无限层级树状页面构建工具，支持用拖拽的方式实现数据的层级、排序等操作
- 内置 40+ 种常用页面组件（如图表、下拉菜单、Tab卡片、提示工具、提示卡片等）
- Section功能（类似`Wordpress`的过滤器Filter）
- 异步文件上传表单，支持分块上传


## 与Laravel Admin的异同

`Dcat Admin`是基于`Laravel Admin`二次开发而成的后台构建工具，整体风格与`Laravel Admin`一脉相承，只是在功能细节上做了大量的调整。


调整：
- 采用[vuexy](https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/landing/)(`bootstrap4`+`jQuery3`)构建前端页面
- 使用`PJAX`构建无刷新页面，并且支持前端资源按需加载，开发者无需再担心安装组件过多会影响页面加载速度
- 采用松耦合的页面构建与数据操作设计，构建页面不再需要关心数据操作接口的具体实现
- 调整了表单提交方式，改为`ajax`提交
- 调整了代码生成器，支持根据已有数据表一键生成增删改查页面
- 调整了多语言翻译功能，使用更简单
- 调整了权限功能，支持分层级以及排序
- 调整了扩展系统，支持页面管理
- ...

新增：
- 新增表单弹窗功能，仅需增加数行代码就可以构建一个非Iframe表单弹窗
- 新增快速构建无菜单栏页面功能
- 新增`SelectResource`表单，可以在弹窗中选择表格数据
- 新增AJAX提价表单以及表单前端验证功能
- 新增文件异步上传组件，支持分块上传、批量上传、上传进度条等
- 新增了表格过滤器右侧滑动面板布局
- 新增了表格字段值过滤功能
- 新增了分步表单
- 新增`section`功能（与`wordpress`的`add_filter`功能类似）
- 新增树形表格功能，可分页显示大批量的层级结构数据
- 新增双表头表格功能，仅需增加数行代码即可构建出双表头表格
- 新增了多种实用的页面组件，如图表、下拉菜单、markdown、checkbox等等
- 新增`Tree`表单
- 新增通过数组添加菜单的功能，支持绑定权限和角色
- 新增通过数组添加菜单功能
- 新增菜单缓存功能
- ...

## 交流

**QQ群** 704661955
