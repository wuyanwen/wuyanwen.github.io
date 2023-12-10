---
title: 从 0 到1 搭建 Vite 3 + Vue 3 前端工程化项目
date: 2023-11-18 09:20:29
tags: [Vite 3,Vue 3]
---
Vue 3 正式版已经发布有一段时间了，随着 Vite 脚手架注定成为下一代前端工具链，许多用户都想基于 Vite 来构建 Vue 项目，如果想基于 Vite 构建 Vue 3 项目，社区模板完全满足您的需求，如果想构建 Vite 3 + Vue 3 + JavaScript 项目，那社区模板不太能满足您的需求，因为社区模板提供 Vue 3 项目几乎是基于 Vite 2 + TypeScript 构建，对于不熟悉 TypeScript 语言的用户不是很友好，因此接下来从 0 开始手把手带大家搭建一套规范的 Vite 3 + Vue 3 + JavaScript 前端工程化项目环境。

## 本文章篇幅较长，从以下几个方面展开：

>  基础搭建
>  代码规范
>  提交规范
>  自动部署

技术栈
⚡️ Vite 3 - 构建工具（就是快！）
🖖 Vue 3 - 渐进式 JavaScript 框架
🚦 Vue Router - 官方路由管理器
📦 Pinia - 值得你喜欢的 Vue Store
🎨 Less - CSS 预处理器
🔗 Axios - 一个基于 promise 的网络请求库，可以用于浏览器和 node.js
🧰 Husky + Lint-Staged - Git Hook 工具
🛡️ EditorConfig + ESLint + Prettier + Stylelint - 代码规范
🔨 Commitizen + Commitlint - 提交规范
💡 GitHub Actions - 自动部署

## 基础搭建

> 构建项目雏形

确保你安装了最新版本的 Node.js，然后在命令行中运行以下命令：

<!--more-->

```
# npm 6.x
npm create vite@latest vite-vue-js-template --template vue

# npm 7+, extra double-dash is needed:
npm create vite@latest vite-vue-js-template -- --template vue

# yarn
yarn create vite vite-vue-js-template --template vue

# pnpm
pnpm create vite vite-vue-js-template --template vue
```

这一指令将会安装并执行 create-vite，它是一个基本模板快速启动项目工具。

在项目被创建后，通过以下步骤安装依赖并启动开发服务器：

```
# 打开项目
cd <your-project-name>

# 安装依赖
npm install

# 启动项目
npm run dev
```
## Vite 基础配置

Vite 配置文件 vite.config.js 位于项目根目录下，项目启动时会自动读取。

本项目针对公共基础路径、自定义路径别名、服务器选项、构建选项等做了如下基础配置：

```
import { defineConfig } from 'vite';
import { resolve } from 'path';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    base: './',
    plugins: [
      vue(),
    ],
    resolve: {
      alias: {
        '@': resolve(__dirname, './src') ,
      },
    },
    server: {
      // 是否开启 https
      https: false,
      // 端口号
      port: 3000,
      // 监听所有地址
      host: '0.0.0.0',
      // 服务启动时是否自动打开浏览器
      open: true,
      // 允许跨域
      cors: true,
      // 自定义代理规则
      proxy: {},
    },
    build: {
      // 设置最终构建的浏览器兼容目标
      target: 'es2015',
      // 构建后是否生成 source map 文件
      sourcemap: false,
      //  chunk 大小警告的限制（以 kbs 为单位）
      chunkSizeWarningLimit: 2000,
      // 启用/禁用 gzip 压缩大小报告
      reportCompressedSize: false,
    },
});
```
关于 Vite 更多配置项及用法，请查看 Vite 官网 vitejs.dev/config.

## 规范目录结构

```
├── dist/
└── src/
    ├── api/                       // 接口请求目录
    ├── assets/                    // 静态资源目录
    ├── common/                    // 通用类库目录
    ├── components/                // 公共组件目录
    ├── router/                    // 路由配置目录
    ├── store/                     // 状态管理目录
    ├── style/                     // 通用样式目录
    ├── utils/                     // 工具函数目录
    ├── views/                     // 页面组件目录
    ├── App.vue
    ├── main.js
├── tests/                         // 单元测试目录
├── index.html
├── jsconfig.json                  // JavaScript 配置文件
├── vite.config.js                 // Vite 配置文件
└── package.json
```

## 集成 Vue Router 路由工具

> 安装依赖

```
npm i vue-router@4
```

> 创建路由配置文件

在 src/router 目录下新建 index.js 文件与 modules 文件夹

```
└── src/
    ├── router/
    	├── modules/  // 路由模块
        ├── index.js  // 路由配置文件
```
关于路由表，建议根据功能的不同来拆分到 modules 文件夹中，好处是：

方便后期维护

减少 Git 合并代码冲突可能性

```
export default [
  {
    path: '/',
    name: 'home',
    component: () => import('@/views/HomeView.vue'),
  },
  {
    path: '/about',
    name: 'about',
    component: () => import('@/views/AboutView.vue'),
  },
];
```

```
import { createRouter, createWebHistory } from 'vue-router';

import baseRouters from './modules/base';

const routes = [...baseRouters];

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes,
  scrollBehavior() {
    return {
      el: '#app',
      top: 0,
      behavior: 'smooth',
    };
  },
});

export default router;
```

根据路由配置的实际情况，需要在 src 下创建 views 目录，用来存储页面组件。

挂载路由配置

在 main.js 文件中挂载路由配置

```
import { createApp } from 'vue';

import App from './App.vue';
import router from './router';

createApp(App).use(router).mount('#app');
```

## 集成 Pinia 全局状态管理工具

> 安装依赖

```
npm i pinia
```
创建仓库配置文件
在 src/store 目录下新建 index.js 文件与 modules 文件夹

```
└── src/
    ├── store/
    	├── modules/  // 仓库模块
        ├── index.js  // 仓库配置文件
```

```
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 1,
  }),
  actions: {
    accumulate() {
      this.count++;
    },
  },
});
```

```
import { createPinia } from 'pinia';

const store = createPinia();

export default store;

export * from './modules/counter';
```

开发中需要将不同功能所对应的状态，拆分到不同的 modules，好处如同路由模块一样。

挂载 Pinia 配置

在 main.js 文件中挂载 Vuex 配置

```
import { createApp } from 'vue';

import App from './App.vue';
import store from './store';
import router from './router';

createApp(App).use(router).use(store).mount('#app');
```

## 集成 Axios HTTP 工具

> 安装依赖

```
npm i axios
```

> 请求配置

在 utils 目录下创建 request.js 文件，配置好适合自己业务的请求拦截和响应拦截：

```
└── src/
	├── api  // 接口
    ├── utils/
        ├── request.js  // axios 请求库二次封装
```

```
import axios from 'axios';

// 创建请求实例
const instance = axios.create({
  baseURL: '/api',
  // 指定请求超时的毫秒数
  timeout: 1000,
  // 表示跨域请求时是否需要使用凭证
  withCredentials: false,
});

// 前置拦截器（发起请求之前的拦截）
instance.interceptors.request.use(
  (config) => {
    /**
     * 在这里一般会携带前台的参数发送给后台，比如下面这段代码：
     * const token = getToken()
     * if (token) {
     *  config.headers.token = token
     * }
     */
    return config;
  },
  (error) => {
    return Promise.reject(error);
  },
);

// 后置拦截器（获取到响应时的拦截）
instance.interceptors.response.use(
  (response) => {
    /**
     * 根据你的项目实际情况来对 response 和 error 做处理
     * 这里对 response 和 error 不做任何处理，直接返回
     */
    return response;
  },
  (error) => {
    const { response } = error;
    if (response && response.data) {
      return Promise.reject(error);
    }
    const { message } = error;
    console.error(message);
    return Promise.reject(error);
  },
);

// 导出常用函数

/**
 * @param {string} url
 * @param {object} data
 * @param {object} params
 */
export function post(url, data = {}, params = {}) {
  return instance({
    method: 'post',
    url,
    data,
    params,
  });
}

/**
 * @param {string} url
 * @param {object} params
 */
export function get(url, params = {}) {
  return instance({
    method: 'get',
    url,
    params,
  });
}

/**
 * @param {string} url
 * @param {object} data
 * @param {object} params
 */
export function put(url, data = {}, params = {}) {
  return instance({
    method: 'put',
    url,
    params,
    data,
  });
}

/**
 * @param {string} url
 * @param {object} params
 */
export function _delete(url, params = {}) {
  return instance({
    method: 'delete',
    url,
    params,
  });
}

export default instance;
```

## 集成 EditorConfig 配置

EditorConfig主要用于统一不同 IDE 编辑器的编码风格。

在项目根目录下添加 .editorconfig 文件：

```
# 表示是最顶层的 EditorConfig 配置文件
root = true

# 表示所有文件适用
[*]
# 缩进风格（tab | space）
indent_style = space
# 控制换行类型(lf | cr | crlf)
end_of_line = lf
# 设置文件字符集为 utf-8
charset = utf-8
# 去除行首的任意空白字符
trim_trailing_whitespace = true
# 始终在文件末尾插入一个新行
insert_final_newline = true

# 表示仅 md 文件适用以下规则
[*.md]
max_line_length = off
trim_trailing_whitespace = false

# 表示仅 ts、js、vue、css 文件适用以下规则
[*.{ts,js,vue,css}]
indent_size = 2
```

很多 IDE 中会默认支持此配置，但是也有些不支持，如：VSCode、Atom、Sublime Text 等。

具体列表可以参考官网，如果在 VSCode 中使用需要安装 EditorConfig for VS Code 插件。

## 集成 ESLint 配置

ESLint[25] 是针对 EScript 的一款代码检测工具，它可以检测项目中编写不规范的代码，如果写出不符合规范的代码会被警告。

由此我们就可以借助于 ESLint 强大的功能来统一团队的编码规范。

安装依赖
`ESLint` - ESLint 本体
`eslint-define-config` - 改善 ESLint 规范编写体验
`eslint-plugin-vue` - 适用于 Vue 文件的 ESLint 插件
`eslint-config-airbnb-base` - Airbnb JavaScript 风格指南
`eslint-plugin-import` - 使用 eslint-config-airbnb-base 时必须安装的前置插件
`vue-eslint-parser` - 使用 eslint-plugin-vue 时必须安装的 ESLint 解析器

```
npm i eslint eslint-define-config eslint-config-airbnb-base eslint-plugin-import eslint-plugin-vue vue-eslint-parser -D
```

## 创建 ESLint 配置文件

在项目根目录创建 .eslintrc.js 文件，并填入以下内容：

```
onst { defineConfig } = require('eslint-define-config');

module.exports = defineConfig({
  root: true,
  env: {
    browser: true,
    node: true,
    jest: true,
    es6: true,
  },
  plugins: ['vue'],
  parser: 'vue-eslint-parser',
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    allowImportExportEverywhere: true,
    ecmaFeatures: {
      jsx: true,
    },
  },
  extends: [
    'airbnb-base',
    'eslint:recommended',
    'plugin:vue/vue3-essential',
    'plugin:vue/vue3-recommended',
    'plugin:prettier/recommended',
  ],
  rules: {
    // 禁止使用多余的包
    'import/no-extraneous-dependencies': 0,
    // 确保在导入路径内一致使用文件扩展名
    'import/extensions': 0,
    // 确保导入指向可以解析的文件/模块
    'import/no-unresolved': 0,
    // 首选默认导出导入/首选默认导出
    'import/prefer-default-export': 0,
    // 要求使用 let 或 const 而不是 var
    'no-var': 'error',
    // 禁止使用 new 以避免产生副作用
    'no-new': 1,
    // 禁止变量声明与外层作用域的变量同名
    'no-shadow': 0,
    // 禁用 console
    'no-console': 0,
    // 禁止标识符中有悬空下划线
    'no-underscore-dangle': 0,
    // 禁止在可能与比较操作符相混淆的地方使用箭头函数
    'no-confusing-arrow': 0,
    // 禁用一元操作符 ++ 和 --
    'no-plusplus': 0,
    // 禁止对 function 的参数进行重新赋值
    'no-param-reassign': 0,
    // 禁用特定的语法
    'no-restricted-syntax': 0,
    // 禁止在变量定义之前使用它们
    'no-use-before-define': 0,
    // 禁止直接调用 Object.prototypes 的内置属性
    'no-prototype-builtins': 0,
    // 禁止可以在有更简单的可替代的表达式时使用三元操作符
    'no-unneeded-ternary': 'error',
    // 禁止重复模块导入
    'no-duplicate-imports': 'error',
    // 禁止在对象中使用不必要的计算属性
    'no-useless-computed-key': 'error',
    // 强制使用一致的缩进
    indent: ['error', 2],
    // 强制使用骆驼拼写法命名约定
    camelcase: 0,
    // 强制类方法使用 this
    'class-methods-use-this': 0,
    // 要求构造函数首字母大写
    'new-cap': 0,
    // 强制一致地使用 function 声明或表达式
    'func-style': 0,
    // 强制一行的最大长度
    'max-len': 0,
    // 要求 return 语句要么总是指定返回的值，要么不指定
    'consistent-return': 0,
    // 强制switch要有default分支
    'default-case': 2,
    // 强制剩余和扩展运算符及其表达式之间有空格
    'rest-spread-spacing': 'error',
    // 要求使用 const 声明那些声明后不再被修改的变量
    'prefer-const': 'error',
    // 强制箭头函数的箭头前后使用一致的空格
    'arrow-spacing': 'error',
  },
    overrides: [
    {
      files: ['*.vue'],
      rules: {
        // 要求组件名称总是多个单词
        'vue/multi-word-component-names': 0,
      },
    },
  ],
});
```

## 创建 ESLint 过滤规则

在项目根目录添加一个 .eslintignore 文件，内容如下：

```
dist
node_modules
!.prettierrc.js
```

## 集成 Prettier 配置

Prettier 是一款强大的代码格式化工具，支持 JavaScript、TypeScript、CSS、SCSS、Less、JSX、Angular、Vue、GraphQL、JSON、Markdown 等语言，基本上前端能用到的文件格式它都可以搞定，是当下最流行的代码格式化工具。

```
npm i prettier -D
```

## 创建 Prettier 配置文件

```
module.exports = {
  // 一行最多 120 字符
  printWidth: 120,
  // 使用 2 个空格缩进
  tabWidth: 2,
  // 不使用缩进符，而使用空格
  useTabs: false,
  // 行尾需要有分号
  semi: true,
  // 使用单引号
  singleQuote: true,
  // 对象的 key 仅在必要时用引号
  quoteProps: 'as-needed',
  // jsx 不使用单引号，而使用双引号
  jsxSingleQuote: false,
  // 末尾需要有逗号
  trailingComma: 'all',
  // 大括号内的首尾需要空格
  bracketSpacing: true,
  // jsx 标签的反尖括号需要换行
  jsxBracketSameLine: false,
  // 箭头函数，只有一个参数的时候，也需要括号
  arrowParens: 'always',
  // 每个文件格式化的范围是文件的全部内容
  rangeStart: 0,
  rangeEnd: Infinity,
  // 不需要写文件开头的 @prettier
  requirePragma: false,
  // 不需要自动在文件开头插入 @prettier
  insertPragma: false,
  // 使用默认的折行标准
  proseWrap: 'preserve',
  // 根据显示样式决定 html 要不要折行
  htmlWhitespaceSensitivity: 'css',
  // vue 文件中的 script 和 style 内不用缩进
  vueIndentScriptAndStyle: false,
  // 换行符使用 lf
  endOfLine: 'lf',
  // 格式化嵌入的内容
  embeddedLanguageFormatting: 'auto',
  // html, vue, jsx 中每个属性占一行
  singleAttributePerLine: false,
};
```

# Commit Message 格式规范

值	描述
feat	新增功能
fix	修复问题
docs	文档变更
style	代码格式（不影响功能，例如空格、分号等格式修正）
refactor	代码重构
perf	改善性能
test	测试
build	变更项目构建或外部依赖（例如 scopes: webpack、gulp、npm 等）
ci	更改持续集成软件的配置文件和 package 中的 scripts 命令，例如 scopes: Travis, Circle 等
chore	变更构建流程或辅助工具
revert	代码回退


