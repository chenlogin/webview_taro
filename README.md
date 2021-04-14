# webview_taro
* https://docs.taro.zone/docs/guide
* 创建全新的taro项目
    * 安装 Taro CLI
        * npm i -g @tarojs/cli
    * taro init
    * 查看 Taro 版本信息
        * npm info @tarojs/cli
    * 诊断项目的依赖、设置、结构，以及代码的规范是否存在问题
        * taro doctor
    * 查看 CLI 相关配置
        * taro config list --json
    * 最小版本的 Taro 项目包括以下文件![avatar](/taroTree.png)
* app.js，每一个 Taro 项目都有一个入口组件和一个入口配置，可以在入口组件中设置全局状态/全局生命周期
    * 在 Taro 中使用 Vue，入口组件必须导出一个 Vue 组件，在入口组件中我们可以设置全局状态或访问小程序入口实例的生命周期
    * app.config.js 的默认导出就是小程序的全局配置
* 页面组件是每一项路由将会渲染的页面，Taro 的页面默认放在 src/pages 中，每一个 Taro 项目至少有一个页面组件
* Taro 中尺寸单位建议使用 px、 百分比 %，Taro 默认会对所有单位进行转换。在 Taro 中书写尺寸按照 1:1 的关系来进行书写，即从设计稿上量的长度 100px，那么尺寸书写就是 100px，当转成微信小程序的时候，尺寸将默认转换为 100rpx，当转成 H5 时将默认转换为以 rem 为单位的值。如果你希望部分 px 单位不被转换成 rpx 或者 rem ，最简单的做法就是在 px 单位中增加一个大写字母，例如 Px 或者 PX 这样，则会被转换插件忽略。结合过往的开发经验，Taro 默认以 750px 作为换算尺寸标准，如果设计稿不是以 750px 为标准，则需要在项目配置 config/index.js 中进行设置，例如设计稿尺寸是 640px，则需要修改项目配置 config/index.js 中的 designWidth 配置为 640
* 多端同步调试：outputRoot: `dist/${process.env.TARO_ENV}
* 如果渲染从远程得来的庞大数据，或者列表渲染的 DOM 结构异常复杂，这就可能会产生性能问题。为了解决这一问题，Taro 内置了虚拟列表（VirtualList）功能，比起全量渲染所有列表数据，我们只需要渲染当前可视区域(visable viewport)的视图
* 渲染一个存在本地的巨大列表，但如果把应用放在真机小程序中，尤其是一些性能不高的真机中，切换到此页面的时间可能会比较长，会有一段白屏时间。这是由于 Taro 的渲染机制导致的：在页面初始化时，原生小程序可以从本地直接取数据渲染，但 Taro 会把初始数据通过 React/Vue 渲染成一颗 DOM 树，然后将这颗 DOM 树序列化之后交给小程序渲染。也就是说，比起原生小程序 Taro 会在页面初始化时多一次调用 setData 函数的支出——而大部分小程序的性能问题是 setData 数据过大导致的。为了解决这个问题，Taro 引入了一种名为预渲染（Prerender）的技术，和服务端渲染一样，在 Taro CLI 直接将要渲染的页面转换为 wxml 字符串，这样就获得了与原生小程序一致甚至更快的速度。
* 在 Taro 应用中，所有 Java(Type)Script 都是通过 babel.config.js 配置的，具体来说是使用 babel-prest-taro 这个 Babel 插件编译的。默认而言 Taro 会兼容所有 @babel/preset-env 支持的语法，并兼容到 iOS 9 和 Android 5，如果你不需要那么高的兼容性，或者不需要某些 ES2015+ 语法支持，可以自行配置 babel.config.js 达到缩小打包体积效果。
* Taro 使用 Webpack 作为内部的打包系统，有时候当我们的业务代码使用了 require 语法或者 import default 语法，Webpack 并不能给我们提供 tree-shaking 的效果。在这样的情况下我们通过 webpack-bundle-analyzer 来分析我们依赖打包体积，这个插件会在浏览器打开一个可视化的图表页面告诉我们引用各个包的体积。