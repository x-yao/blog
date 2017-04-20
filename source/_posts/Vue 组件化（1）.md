title: Vue 组件化（1）
categories: 前端
tags:
  - vue
---

## Vue 组件化（1）

### 前言
> 组件化是永恒不变的主题

现在商户后台的整体设计视觉区域完善，组件化的进程也需要加紧完成了。如何去做好是一个比较难的问题。

目标主要是几个方面：

<!-- more -->
* ~~支持按需加载~~
* 攻克几个比较困难的组件（autocomplate，validator，select，datatable，dateselect）
* 完善基础组件（loading，dialog，toast，transition）
* 完成文档demo页面以及首页
* 第三方非webpack项目可以使用

### 按需加载
> 重要但非必要

antd和element的具体思路就是组件作为npm包，除了拥有集合所有组件的index文件外各个组件可以单独作用并打包出来然后通过

```javascript
	import Button from 'ui/lib/button';
```
	
这样的方式进行单独加载。或者使用`antd`的 [babel-plugin-import](https://github.com/ant-design/babel-plugin-import) 做语法转换。

或许这个时候想到了**webpack2**的`tree-shaking`。不过遗憾的是，现在的`tree-shaking`不支持多入口下的完美shaking，所有其他入口使用过但是当前入口未使用的都会打包进来，而且如果这么做需要未编译的npm包且项目webpack不忽视`node_modules`（这样做成本太高）

不过上面我说这个重要但非必要，考虑到现有商户后台组件化组件量不是很多，没有antd那样全面，最后文件不会特别巨大。而且对于某些复杂的页面特别是spa而言，基本上绝大部分组件都会用到，如果使用到按需加载也就是意味着所有地方都需要按需加载，这样工作量也会更大。

不过，现在的商户后台并不是SPA这点还是很伤心，按需加载的需求还是很重要的，所以还是要尽量，或者区分基础组件与复杂组件作为区分加载，这样可能是比较好的方法和思路。