---
title: 小程序 + canvas
date: 2021-11-30 13:34:47
tags:
- 小程序
- canvas
---
```wxml
 <canvas canvas-id="pageCanvas"></canvas>
```

### 小程序画图
> 图片全屏适配，高度计算
```javascript
let statusBarHeight = 0;
await wx.getSystemInfo({
  success: (res) => {
    // 获取手机的信息，获取顶部状态条的高度
    statusBarHeight = res.statusBarHeight + 44 // 44 为导航条的高度
  }
})
```

> 绘制图像到画布
```javascript
let canvasContext = wx.createCanvasContext('pageCanvas')
// imageSource 要绘制的图片资源，网络资源要通过 getImageInfo / downloadFile 下载
wx.getImageInfo({
  src: 'https://res.wx.qq.com/wxdoc/dist/assets/img/draw-image.22401fa6.png',
  success: function (res) {
    const { width, height, path } = res;
    // 计算图片的高
    let imgHeight = canvasWidth / width * height
    canvasContext.drawImage(path, 0, statusBarHeight, canvasWidth, imgHeight)
    canvasContext.draw()
  }
})
```

> 绘制圆角矩形，参考 [圆角矩形绘图原理](https://blog.csdn.net/weixin_44953227/article/details/111561677)

![img.png](/images/miniPrograme/canvas-to-rectangle.gif)
```javascript
/**
 * 
 * @param context
 * @param x 圆角矩形 x 轴起始位置
 * @param y 圆角矩形 y 轴起始位置
 * @param r 圆角半径
 * @param w 矩形的宽
 * @param h 矩形的高
 * 单位相素 px
 * context.arcTo 根据控制点和半径绘制圆弧路径
 * context.arcTo(第一个控制点x，第一个控制点y, 第二个控制点x, 第二个控制点y,半径)
 */
function drawRoundedRectangle(context, x, y, r, w, h) {
  context.moveTo(x+r, y)
  context.arcTo(x+w, y, x+w, y+h, r)
  context.arcTo(x+w, y+h, x, y+h, r)
  context.arcTo(x, y+h, x, y, r)
  context.arcTo(x, y, x+w, y, r)
}
```
