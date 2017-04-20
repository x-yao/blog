title: rgba颜色转化
categories: 前端
tags:
  - rgba
---

## rgba颜色转化

### 前言

有的时候拿到设计稿会发现设计师在对一些非透明的元素标注时使用rgba的颜色，在实际使用中需要把它们转换成十六进制颜色或者其他非含有透明度色值。

<!-- more -->
### 转化

rgba是在rgb色值的基础上添加了透明度`a`通道的颜色，它通常等同于相应的rgb颜色外加`opacity`属性作为替代`a`通道。所以如果需要把它转化成非透明颜色就**需要考虑到它的底色作为一个整合**。

我们一般拿底色作为一个底色，这个底色一般使用白色或者黑色。然后通过下面的公式来叠加底色与rgba源色算出叠加后的rgb色值。

```javascript
		Source => Target = (BGColor + Source) =
		Target.R = ((1 - Source.A) * BGColor.R) + (Source.A * Source.R)
		Target.G = ((1 - Source.A) * BGColor.G) + (Source.A * Source.G)
		Target.B = ((1 - Source.A) * BGColor.B) + (Source.A * Source.B)
```
		
比如，底色为白色，`BGColor`的R,G,B通道都是**255**，源色为`rgba(50,240,100,.4)`，那么公式核算出颜色就是`rgb(173,249,193)`。如果底色是黑色，则核算出的颜色为`rgb(20,96,40)`。

### demo

使用[**color-transform**](https://www.x-yao.com/color-transform.html)可以进行rgb，rgba，十六进制色值的相互转化，比如rgba可以转化成十六进制和rgb，十六进制和rgb之间可以相互转化。

**github地址**:[https://github.com/x-yao/color-transform](https://github.com/x-yao/color-transform)