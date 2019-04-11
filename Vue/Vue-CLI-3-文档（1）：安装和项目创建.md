[TOC]

# Vue CLI 3 文档（1）：安装和项目创建

之前正好遇到新的项目，便尝使用`Vue CLI 3`来进行构建，玩了小半年之后抽空整理下文档。

## 简介

Vue CLI 作为 Vue 的官方脚手架，降低了开发者对于 webpack 的配置成本，便于快速构建项目。具体的介绍在[官方文档](https://cli.vuejs.org/zh/guide/)中写的十分详细，此处就不唠叨。

用一个通俗的比喻来说，把项目作为一份晚餐：食材（vue）虽都有，可但油盐酱醋（webpack）怎么配比，并非一下就能掌握。而 Vue CLI 就像是成品大礼包，198、298、398自由搭配，营养均衡开袋即食。并且附带料包自由调整（配置），也能加热享用（可升级）。并且 Vue 是渐进式框架，需求（基础插件）无法满足时，可单独外卖（Plug-in）加餐。
<!--more-->
## 说明

Vue CLI 要求 [Node.js](https://nodejs.org/) 8.9 或更高版本 (推荐 8.11.0+)
已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，需要先通过`npm uninstall vue-cli -g`或`yarn global remove vue-cli`卸载后再安装。

## 安装

通过以下2种方式安装（我个人选择`yarn`）：

``` php
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

安装成功后，可以执行命令来查看版本是否正确：
``` php
vue -V
```

## 项目创建

执行创建指令，并填写项目名称：`vue-demo`
``` php
vue create vue-demo
```

选择配置类型：`Manually select features`
> 上下键移动、回车键确定
> 其中`My First Preset`为我之前保存过的配置方案，初次安装不会出现（后文会涉及到说明）。

``` php
Vue CLI v3.0.0
? Please pick a preset:
  My First Preset (vue-router, vuex, sass, babel, eslint) // 已保存过的配置
  default (babel, eslint)   // 默认配置（包含：babel、eslint）
> Manually select features  // 手动配置
```

选择项目中所需要的特性（依自身项目而定）：`Babel`、`Router`、`Vuex`、`CSS Pre-processors `、`Linter / Formatter`
> 空格键选定、A键全选、I键反选

``` php
? Check the features needed for your project:
>(*) Babel                              // Babel 编译
 ( ) TypeScript                         // TypeScript 编译器
 ( ) Progressive Web App (PWA) Support  // PWA 的支持
 (*) Router                             // vue 路由
 (*) Vuex                               // vue 状态管理器
 (*) CSS Pre-processors                 // CSS 预处理器
 (*) Linter / Formatter                 // 代码检测和格式化
 ( ) Unit Testing                       // 单元测试
 ( ) E2E Testing                        // 端对端测试
```

接下来，针对刚才所选特性进行单独配置（我默认把所有选配项说明都罗列出来，如没有选择则自动跳过）：

TypeScript 配置选项：（自动跳过）

``` php
? Use class-style component syntax? (Y/n)                            // 是否使用 class 风格的组件语法
? Use Babel alongside TypeScript for auto-detected polyfills? (Y/n)  // 是否使用 babel 做转义
```

路由的配置选项：`Y`/`回车`
``` php
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) // 是否使用 history 模式作为 Vue 的路由（n）
```

选择 CSS 预处理器方案：`SCSS/SASS`
``` php
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
> SCSS/SASS
  LESS
  Stylus
```

选择代码检测和格式化方案：`ESLint + Standard config`
``` php
? Pick a linter / formatter config:
  ESLint with error prevention only  // 只检测错误
  ESLint + Airbnb config             // ESLint + Airbnb 规范风格
> ESLint + Standard config           // ESLint + standardJs 规范风格
  ESLint + Prettier                  // ESLint + Prettier 代码格式化工具
```

选择何时进行代码检测：`Lint on save`
``` php
? Pick additional lint features: 
>(*) Lint on save            // 保存时进行检测
 ( ) Lint and fix on commit  // fix 和 commit 时候检查
```

选择单元测试方案：（自动跳过）
``` php
? Pick a unit testing solution:
> Mocha + Chai
  Jest
```

选择端对端测试方案：（自动跳过）
``` php
? Pick a E2E testing solution: (Use arrow keys)
> Cypress (Chrome only)
  Nightwatch (Selenium-based)
```

选择（`Babel`、`PostCSS`、`ESLint`）自定义配置的存放位置：
``` php
? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.?
> In dedicated config files // 单独保存各自配的置文件
  In package.json           // 放在 package.json 内
```

是否保存当前配置：`Y`/`回车`
``` php
? Save this as a preset for future projects? (y/N)  // 是否保存现在的配置，作为未来项目的预配置
```

输入保存的配置名称：`My First Preset`
> 保存配置之后，在以后执行创建指令时会多出此选项，选择该配置便能跳过配置直接安装。

``` php
? Save preset as: My First Preset  // 输入存档名称
```

开始项目初始化：
``` php
Vue CLI v3.0.0
✨  Creating project in e:\Vue\vue-demo.
🗃  Initializing git repository...
⚙  Installing CLI plugins. This might take a while...
...
```


初始化完毕后，可以通过命令启动项目：
``` js
yarn serve
```

> 如果之前有接触过`Vue CLI 1.x/2.x`版本，便会发现在的项目结构变的更为简洁，少去了`bulid`和`config`两个存放配置的目录，主要为了让开发者更专注与开发，减轻开发对环境配置的操作。

此时查看根目录`package.json`文件，能够看到`scripts`配置中除了`serve`，还包含生产构建（yarn build）、代码规范检测（yarn lint）这两个执行命令。

``` js
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint"
}
```

至此，一套基于`Vue CLI 3`构建的项目，雏形已完成。

参考资料：
[Vue CLI 3](https://cli.vuejs.org/zh/guide/)
[错过了Vue CLI2，还要错过Vue CLI3?](http://www.10tiao.com/html/780/201808/2650587864/1.html)