# Sandwich

一个落地页模板项目，包含可视化广告页面编辑器、站点和渲染器。

[https://znlbwo.github.io/sandwich/editor/](https://znlbwo.github.io/sandwich/editor/)

## 背景

我司因业务发展，需在各大平台投放大量广告收集流量，因此需要制作大量广告页面。
如果纯靠投入前端开发人员协助投放端业务人员进行广告页面开发，别说 996，就算是 9127 都无法跟上业务人员节奏。

**一切都是因为利益...**

为什么这么说？因为投放端业务人员的业绩，与获取流量的数量和质量直接挂钩。就是说如果你能以最少的成本获取最多且最有质量的流量，那么你这个月拿到的 money 就会越多。  
那么如何以最少的成本获取最多且最有质量的流量呢？  
以价格和时间为基准不断调整落地页内容，以达到最优的广告投放效果。

为了以最少的成本获得最多的流量，投放端业务人员会实时根据广告效果调整投放策略。其中一项策略就是调整广告页面展示的内容，也就是说，广告页面的内容需要前端人员配合投放人员进行实时更改，试问这有几个前端能受得了？  
为什么要实时？用户白天上班、上学，娱乐消遣大多集中在晚上那几个小时，如果因为落地页内容的一点小问题，导致流量收集效果较差，如果不能进行实时调整，耽误时间错过高峰期，无论是对个人还是公司，都是损失。

一个可以给不懂技术的业务人员编辑和生成广告页面的需求由此而生。在此之前，广告投放人员制作广告页面，要么是通过广告平台提供的建站工具，比如头条系的橙子建站。要么是通过第三方平台的工具，比如麦客表单。  
无论是上述哪种方式，都有一个致命问题，收集到的流量信息无法直接进入到我司的 CRM 系统。这些第三方工具它们都不提供自定义广告页面表单内容发送接口配置，所以之前的流量信息都是通过人工从第三方广告页面制作平台导出 Excel，再由人工导入到我司 CRM 系统。

上述，使用第三方平台工具会产生如下三个问题：

1. 收集到的流量无法实时分配到负责跟进的业务人员手中，导致成交率降低。
2. 流量数据经由业务人员进行 Excel 导入导出操作，存在数据安全问题，业务人员为了谋取私利可能产生不正当行为。
3. 业务人员需要在好几个甚至几十个广告平台进行切换，查看广告数据，分析广告效果，继而调整广告策略，操作十分繁琐。

以上 1、2 点，做一个建站工具即可，将广告页面收集到的流量通过接口实时传入我司 CRM 系统。第 3 点，需要做广告数据的统计，目前无法完全解决，我们能获取到的数据有限。比如每条广告的价格数据仍在各个广告平台，我们无法进行 ROI 的计算，而且需要对各个第三方广告平台进行数据回传等等，还有许多与业务相关的需求，这里不做赘述。所幸第 3 点可以通过运营层面进行优化，提升效率。

此项目由此而生。

_注：此项目不包含任何我司业务内容_

## 怎么做？

经过跟业务人员的沟通，广告页面的内容基本以图片 + 表单为主，辅助一些活动类型组件，比如计数器、报名用户展示等等。功能方面其实都还好说，难点在交互，接到这个任务的时候，就已经参考了头条的橙子建站和麦客表单，心里大概预估了两种类型的广告页面编辑器开发所需要的时间。最后协商的结果就是两个字：“要快”，所以咱们就先按简单的来做。

简单来说，一个广告页面就是一份 JSON 格式的配置内容，围绕这份 JSON 配置内容，需要做以下 3 件事情。

1. 可视化广告页面编辑器：广告投放人员通过它生成和修改这个 JSON 配置，完成之后将其发布，生成一个唯一 ID 加入到广告页面站点的 URL 查询参数；
2. 广告页面站点：通过 URL 查询参数中的唯一 ID 查询到对应的 JSON 配置，给到渲染器展示广告页面内容；
3. 渲染器：接收一份符合条件的 JSON 配置，渲染出页面内容；

<!-- TODO 图片 -->

渲染器是独立的，只负责渲染，不处理业务逻辑，编辑器和站点都依赖它来渲染内容。

## 独立开发和构建

实际项目中，我将可视化广告页面编辑器、广告页面站点和渲染器划分为三个项目，可视化广告页面编辑器和广告页面站点通过 webpack alias 的方式引入渲染器。

在此项目中，并没有将上述三者在项目层面进行划分，而是在模块层面进行划分，通过命令和脚本的方式做到独立开发和构建。  
通过约定式目录结构，在命令中传入不同的项目名称，自动找到对应的入口，执行本地开发或构建服务。在环境变量中添加当前项目名称，提供给 webpack output.path（vue.config.js 内配置项是 outputDir）在构建时将构建内容输出到不同目录。

相关代码可查看 scripts 目录、package.json 和 vue.config.js。

其实更偏向于通过模块的方式进行划分，通过项目层面进行划分会有一些弊端：

1. 每个项目的配置和依赖大多都是重复的；
2. 版本迭代可能涉及到多个项目，一个功能的改动经常需要在多个项目进行分支切换、代码提交操作，略显繁琐；

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Lints and fixes files

```
yarn lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

<!-- ## 记录

### [commitlint](https://github.com/conventional-changelog/commitlint)

### [prettier](https://prettier.io/)

### 脚本

环境变量作用

VUE_CLI_CONTEXT 指定 vue.config.js 上下文位置
VUE_APP_ENV 指定运行时环境
VUE_APP_VERSION 指定当前版本号

### svg 图标

[可编辑的 SVG 图标系统](https://cn.vuejs.org/v2/cookbook/editable-svg-icons.html)
[svg-sprite-loader](https://github.com/JetBrains/svg-sprite-loader)

### vscode jsconfig.json 和 工作区设置（gitignore !）

### eslint no-console no-debugger

~~### number-precision 解决数值计算精度问题~~

### 生成随机码

### [vue css 预处理器自动化导入](https://cli.vuejs.org/zh/guide/css.html#%E8%87%AA%E5%8A%A8%E5%8C%96%E5%AF%BC%E5%85%A5)

### 获取图片的原始宽高和元素宽高 -->
