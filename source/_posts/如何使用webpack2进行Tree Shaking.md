title: 如何使用webpack2进行Tree Shaking
categories: 前端
tags:
  - webpack
  - babel
---

## 如何使用webpack2进行Tree Shaking

> 这篇文章设定读者有一定的webpack基础能够使用webpack2进行基本项目配置。所以具体的配置等没有详细说明，着重说明Tree Shaking这一特性在webpack2中的使用。

### Tree Shaking
> 消灭没有用到的代码

**Tree Shaking**这个词最早听说于`rollup.js`，这个功能能够通过基于ES6 modules的静态特性做检测来找出未使用的代码。然后配合`uglifyjs`把无用代码“筛选”掉。

<!-- more -->
比如在如下场景下：

**util.js**

        export const foo = function () {
	        return 1
        }

        export const bar = function () {
	        return 2
        }

        export const foobar = function () {
	        return 3
        }
        
**bar.js**
    
       import {bar} from './util'
       const a = bar();
       
我们使用`foo.js`作为入口文件，自然希望webpack编译后只把`util.js`里面的`bar`变量引入到最终生成的文件，而其他没有用掉的能够被“优化”掉。Tree Shaking就是为了解决上述问题。

最通常的情况就是我们项目中有某个公用方法文件，但是可能不同入口文件能够使用的方法很有限，这样会产生很多无用的方法打包到最后的文件当中。Tree Shaking则能够移除由此产生的大量冗余代码，达到优化的目的。

### 在webpack2中使用Tree Shaking

#### 基础使用
还是以上述两个代码作为例子。设置`bar`作为入口文件，最后编译出的代码如下：

		"use strict";
		const foo = function () {
			return 1
		}
		/* unused harmony export foo */
		
		
		const bar = function () {
			return 2
		}
		/* harmony export (immutable) */ __webpack_exports__["a"] = bar;
		
		
		const foobar = function () {
			return 3
		}
		/* unused harmony export foobar */

可见`foo`,`foobar `这两个变量已经被定义为**unused**而未exports出来，再经过**压缩**后便没有了这两个变量的代码。

#### 配合Babel
> 移除`CommonJS`模块

在实际生产环境当中我们通常使用`babel`来编译模块化代码，但是初次使用会发现当配合`babel`是会导致`Tree Shaking`失效，原因大多是因为**使用了`CommonJS`编译模块**导致webpack基于ES6 modules的静态分析失效。所以我们需要在`babel`配置中**去掉`CommonJS`模块**。
比如使用如下配置：

		plugins: [  
			        'transform-es2015-template-literals',  
			        'transform-es2015-literals',  
			        'transform-es2015-function-name',  
			        'transform-es2015-arrow-functions',  
			        'transform-es2015-block-scoped-functions',  
			        'transform-es2015-classes',  
			        'transform-es2015-object-super',  
			        'transform-es2015-shorthand-properties',  
			        'transform-es2015-computed-properties',  
			        'transform-es2015-for-of',  
			        'transform-es2015-sticky-regex',  
			        'transform-es2015-unicode-regex',  
			        'check-es2015-constants',  
			        'transform-es2015-spread',  
			        'transform-es2015-parameters',  
			        'transform-es2015-destructuring',  
			        'transform-es2015-block-scoping',  
			        'transform-es2015-typeof-symbol',  
			        ['transform-regenerator', {async: false, asyncGenerators: false}]
			    ]

#### 配合多入口文件
> 多入口文件多webpack配置

有些情况下我们的webpack入口文件有多个，一般情况我们会设置多入口文件也就是在`entry`属性中设置一个入口文件列表的map映射。但是在此情况下需要使用Tree Shaking可能遇到一些问题，详见[Webpack2 with mutli-file entry tree-shaking not behave as expected](https://github.com/webpack/webpack/issues/4353)。

根据开发者回答是现阶段限制就是这样，需要使用**多配置文件**进行配置。也就是说，多入口文件相当于如下：

![](https://img.kuaidadi.com/cms/img/upload_6c65d0dcb41eabff3e01245ca8fc671c.png)

换成多配置文件就要替换成如下结构：

![](https://img.kuaidadi.com/cms/img/upload_1b072a8f87a53e0ff08e16d7714b1068.png)

这样就能够达到需求。详见**demo**：[webpack-tree-shaking-demo](https://github.com/x-yao/webpack-tree-shaking-demo)


