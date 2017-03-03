## Vue 2.0升（tang）级（keng）备忘

### 准备工作

* [官方升级文档](https://cn.vuejs.org/v2/guide/migration.html)
* [官方升级迁移工具](https://github.com/vuejs/vue-migration-helper)

### 生命周期

* `ready`，`init`生命周期钩子替换成为`mounted`,`beforeCreate`

### 页面模板

* `v-for`之前遍历数组时的参数顺序是 (index, value)。现在是 (value, index)
* 单个vue组件中Template内必须只包含一个根元素节点
* `{{{}}}`不被使用，由`v-html`替换
* 标签属性中禁止使用`{{}}`，由`v-bind`代替。

### 方法

* $dispatch 和 $broadcast 已经被弃用。父子组件事件传递使用$on
* directive包裹第三方组件方式不被建议且改动工作量极大。
* runtime中（dev环境）初始化Vue方式变更，使用`render`方法

### vuex2

* 初始化stroe方法变化
* `action`中`mutation`参数只能是一个
* 使用`mapGetters`与`mapActions`替换原本vuex方法引入方式