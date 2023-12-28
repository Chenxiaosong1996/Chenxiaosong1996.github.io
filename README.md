
## Scripts
```sh
# 安装
npm run install

# 开发
npm run dev

# 打包
npm run build

# 预览打包的项目，有build后的文件才可正确运行
npm run preview

# lint
npm run lint
```

&nbsp;
## 项目结构

    ├── public
    |   ├── 404.html         - 用来同步index.html内容，以解决 github 不支持单页面应用问题
    │   ├── favicon.ico      - 浏览器tab缩略图
    │   └── assets           - 项目通用的静态资源统一存放于此
    │       ├── images
    │       ├── scripts
    │       └── ...
    ├── scripts
    │   └── ...              - node脚本文件
    ├── src
    │   ├── components       - 公共组件
    │   ├── modules          - 模块：根据业务逻辑划分的业务模块
    │   ├── router           - 路由模块
    │   ├── store            - 全局状态管理器
    │   ├── style            - 全局样式
    │   ├── utils            - 全局配置，包含进程文件、通用辅助方法等
    │   ├── App.vue          - vue根组件
    │   ├── config.js        - 项目配置变量
    │   └── main.js          - 程序入口
    ├── .dev                 - 环境变量，什么环境都会使用，如果只是针对特定环境可以参考 vue 2 中的 env 定义
    ├── .**ignore            - 忽略文件配置
    ├── .*.js                - 插件配置文件
    ├── index.html           - 网页入口文件
    ├── vite.config.js       - vite 框架配置
    └── windi.config.js      - windicss配置

&nbsp;
## 约定

### 1. 模块化开发
按照业务逻辑将项目拆分为模块，降低不同模块之间的代码耦合程度，UI 与业务数据处理分离，业务数据处理与接口分离，充分利用mvvm架构的特性，优化项目结构。
模块统一放在/src/modules中，一个典型模块的目录结构如下：

    [moduleName]
    ├── components           - 业务逻辑内部用组件
    ├── composables          - 使用组合式API时拆分出的各组合式函数
    ├── store                - 存放vuex store
    ├── views                - 业务逻辑页面
    └── ...                  - 其他文件/文件夹
    
其中，store文件夹用于存放vuex store文件，该文件夹名称必须为store，/src/store/index.js中createStore时会遍历modules下各个模块中名为store的文件夹，自动引入。
views文件夹存放该模块的各个页面文件。其中components文件夹存放仅用于该模块的组件，跨模块使用的组件请存放于/src/components下；composables文件夹用于存放使用组合式API拆分的组合式函数。

另，请注意业务逻辑的拆分粒度，逻辑拆分太粗/太细均会给开发带来不便。

&nbsp;
### 2. Element-Plus组件库
项目使用Element-Plus组件库，详见[官网地址](https://element-plus.gitee.io/#/zh-CN)

&nbsp;
### 3. WindiCss(功能类优先的 CSS 框架)
项目推荐使用windicss来编写样式，无需编写任何css/scss样式，直接在模版使用各种预定义的功能类。
#### 关于utility-first CSS framework(功能类优先的 CSS 框架)
目前主流的有两个：[tailwind](https://www.tailwindcss.cn/)和[windicss](https://windicss.org/)(官网可能需要科学上网才可访问)，其中windicss完全兼容tailwind，所以API文档只看tailwind也可以。详细用法见官方文档。

#### 推荐vscode插件：WindiCSS IntelliSense

&nbsp;
### 4. 组合式API(Composition-api)
项目推荐使用组合式API来编写SFC，将逻辑复杂的组件，通过拆分为多个逻辑关注点，保存在不同的组合式函数中，来实现最大化的逻辑拆分和代码复用。具体见[官方文档](https://v3.cn.vuejs.org/guide/composition-api-introduction.html#%E4%BB%80%E4%B9%88%E6%98%AF%E7%BB%84%E5%90%88%E5%BC%8F-api)

&nbsp;
### 5. 通用请求封装 & 接口请求调用方式
项目使用了通用的Axios请求封装方法 http (位于/src/utils/http.js)，该方法自动处理错误提示，并通过各种回调函数处理请求中的各种状态。

接口请求统一使用vuex的action方式调用。
