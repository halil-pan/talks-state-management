---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## State management
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS
css: unocss
---

# 前端状态管理的一些新思考

panhailiang@thoughtworks.com

---

# 什么是状态

<br>

<v-click>

百度百科中对状态的定义：事物在不同时期或转化临界点时表现出来的形态。

</v-click>

<v-click>

通俗一点讲就是一个东西的样子。

</v-click>

---

# 什么是前端状态

<br>

<v-click>

不知道大家有没有一个困惑，对于前端，到底什么是数据，什么是状态？

</v-click>

<v-click>

可以理解为：当网页打开后，由**人机交互**来确定更新并在一定时间内保留的，就是状态，其他都是数据。

</v-click>

---

# 为什么状态需要管理

<br>

<v-click>

状态管理本质上是对于状态读取操作的封装。

封装（管理）是为了统一控制，保证规范读写的手段。

</v-click>

<v-click>

前端应用的复杂度很大程度上由人机交互复杂度决定，引起各种同步异步状态的变更，状态之间还可能存在依赖耦合。

</v-click>

<v-click>

因此为了克服复杂应用状态的开发维护成本，封装管理是必须要考虑的。

</v-click>

---

# 状态管理的发展

<br>

原生和 `jquery` 时期，开发者需要在维护状态的同时考虑视图的更新，此时还没有模块化组件化的思想，状态都存在闭包或者 dom 的 dataset 属性上。

<br>

<v-click>

此时存在的问题：
- 状态定义分散
- 状态传递随意无序
- 难以跟踪定位
- ...

</v-click>

---

# 状态管理的发展

<br>

随着前端需求复杂度提升，jquery 时期的问题逐渐放大，这时 `vue`、`react` 等前端框架出现

前端开发由原本的一把梭撸页面，变成分工开发组件，对齐 props，拼装组件的过程。

组件树中父子传递 props 这种单向数据流方式，解决了 `jquery` 时期令人头痛的问题。

<v-click>

并且这类框架都通过不同的实现手段做到了感知状态变化，自动更新视图，大大降低了开发成本。

更符合“视图是状态的映射”。

</v-click>

---

# 状态管理的发展

<br>

如果所有状态都通过组件层层传递

很纯正，很清晰

但怎么想都觉得不靠谱

组件接收的很多 props 只将其向下传递，并不使用。

<br>

<v-click>

紧接着中心化的状态管理便开始蓬勃，`redux`、`vuex` 都是一样的核心思想：集中状态管理，共享状态。

`dispatch - state - view` 的流程，简单且通用，到今天，中心化的状体管理基本是大型前端应用的第一选择。

</v-click>

---

# 通用问题与特殊问题

<br>

<v-click>

redux、vuex 这些都是优秀的状态管理工具。

但解决的是前端状态管理中的通用问题。

</v-click>

<br>

<v-click>

通用意味着一定会在一些特殊场景下牺牲效率。

</v-click>

---

# 前端状态管理中的一类特殊场景

<br>

场景：有一个区域下拉选框组件，选项 options 是通过接口动态获取。

<img src="https://s1.ax1x.com/2022/10/30/xIhPmV.jpg" width="400" />

---

# 下拉框组件 -- 方案一

<br>

开发一个 `LanguageSelector` 组件

组件内在 mounted 钩子上调用接口，options 数据保存在组件内部状态

---

# 下拉框组件 -- 方案二

<br>

如果一个页面需要展示多个下拉框组件呢

<br>

<v-click>

状态提升（中心化）

</v-click>

<br>

<v-click>

1. 页面根组件触发 dispatch 调用接口
2. 数据保存在 vuex state 中
3. `LanguageSelector` 组件从 vuex 获取 options

当 `LanguageSelector` 渲染存在条件时，该方案实现下，无论 `LanguageSelector` 是否真正渲染，都会发起请求。

</v-click>

---

# 好用吗？

<br>

思考在后续的组件使用过程中会存在哪些问题

<v-click>

options 与页面根组件耦合，读、写位置过于分散。

对于开发者来说，使用 `LanguageSelector` 组件时组要关注页面及子组件的渲染时机，但本身这些可能是不需要关注的。

</v-click>

---

# 整理需求

1. 中心化：避免页面多次使用组件时重复请求
2. 按需请求：仅在需要数据时获取数据
3. 视图更新：适配框架，自动触发视图更新

---

# talk is cheap

---

# 总结

<br>

也是最近在项目上的小思考，小创新，其实像 eventBus 也算是特殊场景下的状态管理。
个人还是挺喜欢挺满意的，希望能够帮到大家。

---
layout: center
class: text-center
---

# Thanks
