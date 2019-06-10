## 微信小程序知识点总结

### 生命周期

* onLoad:页面加载，一个页面只会调用一次，可以获得调用该页面带入的路由参数。
* onShow:页面显示，每次打开都会调用一次。
* onReady:页面初次渲染完成，一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。
* onHide:页面隐藏，底部tab切换时页面隐藏调用
* onUnload:页面卸载，当redirectTo或navigateBack的时候调用。

### 一些注意点

* 不支持跳转外部链接
* text可以解析/n
* 目前不支持识别图中二维码
* 背景图不能用本地图片，可以使用base64
* wx.navigateTo需要跳转的应用内非tabBar的页面的路径
* wx.switchTab跳转到tabBar页面
* wx.reLaunch可以代开任意的页面
* wx.showToast(), icon只支持success和loading，但支持image,且image优先级高于icon
* 小程序button按钮自带边框效果，而且直接设置border无法去掉，需要设置：after样式
* 正常事件绑定使用的时bindtap,但是该方法无法阻止事件冒泡。这样就会触发父元素的事件，小程序提供另一种方法来解决问题，使用catchtap事件替换bindtap即可。

### 小程序中的原生组件

原生组件是由客户端创建的原生组件。由于原生组件脱离WebView渲染流程之外，因此层级最高，非原生组件无论设置多高的层级都无法覆盖原生组件。
有客户端创建的原生组件：camera、canvas、input、live-player、live-pusher、map、textarea、vieo

### 小程序横竖屏的适配问题
通常如果手机设置了允许横屏，同时，小程序也增加了允许横屏的配置，那么手机屏幕旋转时小程序也会随之旋转。
此时我们可以使用如下方式去调整页面的样式

1、media query
2、onResize事件

```
Page({
  onResize(res) {
    res.size.windowWidth // 新的显示区域宽度
    res.size.windowHeight // 新的显示区域高度
  }
})
Component({
  pageLifetimes: {
    resize(res) {
      res.size.windowWidth // 新的显示区域宽度
      res.size.windowHeight // 新的显示区域高度
    }
  }
})
```

3、wx.onWindowResize事件，不推荐。