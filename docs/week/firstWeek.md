![](![](![](2020-05-24-17-41-37.png).png).png)

## vue 中使用 rem 布局

背景：移动端的设备会越来越多，而且会有不同的分辨率。那如何在不同的手机中显示相同的效果呢？也就是我们常说的移动端适配是怎么做到的呢？

常见的方法有很多，比如百分比、flex 布局等。但现在更加常见的是采用 rem 布局的方式

如何在 Vue 项目中使用 rem 呢？

这里记录一下 使用 lib-flexible 和 px2rem-loader 自动转换

- 安装插件

```js
npm install lib-flexible --save
npm install px2rem-loader --save-dev
```

- main.js 中引入

```js
import 'amfe-flexible/index.js'
```

- 在 webpack 的配置中配置 px2rem-loader，我这里是采用 Vue-cli 搭建的，在 `build/utils.js` 文件中，加入如下配置。

```js
  const px2remLoader = {
    loader: 'px2rem-loader',
    options: {
      // 设计稿是 320 px
      remUnit: 32 // 默认换算为1rem为32px，可根据你的原型图修改
    }
  }
```

另外需要在 generateLoaders 函数中加入我们的 px2remLoader 配置，主要的作用是告诉 webpack，处理 CSS 的使用要经过 px2remLoader 

```js
const loaders = options.usePostCSS ? [cssLoader, postcssLoader, px2remLoader] : [cssLoader, px2remLoader]
```

- 在 Vue 组件中使用
这里我用了 Sass，具体的使用如下。注意，我这里设计稿是 320px,，所以 $rem: 320/10rem; 这一句是计算一个 rem 代表多少 px。然后 40/$rem 计算出来实际的 rem 值。

```css
<style scoped lang="scss">
$rem: 320/10rem;

.header {
  width: 100%;
  height: 40/$rem;
  display: flex;
  align-items: center;
  background: #8560A9;
}
```


参考：[vue 中使用 rem 布局的两种方法](https://blog.csdn.net/Robin_star_/article/details/86638138)

## ajax 请求报 415 错误

没有配置 accept 请求头

```js
xhr.setRequestHeader('Accept', 'application/json')
xhr.setRequestHeader('Content-Type', 'application/json')
```

参考：https://stackoverflow.com/questions/11492325/post-json-fails-with-415-unsupported-media-type-spring-3-mvc

## 当作为 background-image 时修改 SVG 填充颜色

背景：我们知道，可以假如我们使用 svg 在 HTML 中的时候，我们可以通过 CSS 修改它的颜色

```
polygon.mystar {
    fill: blue;
}​

circle.mycircle {
    fill: green;
}
```

但是假如是通过 background-image 引入的 SVG 呢？

解决方法：

CSS masks

这个是存在浏览器的兼容性问题的，基本使用如下所示：

```
.icon {
    background-color: red;
    -webkit-mask-image: url(icon.svg);
    mask-image: url(icon.svg);
}
```



## dataLoader 源码的阅读【初步】
1.load 方法通过 getCurrentBatch 返回当前的 Batch，这个方法中又通过 _batchScheduleFn 方法即  getValidBatchScheduleFn 方法处理当前的，如果没有，通过 enqueuePostPromiseJob 方法，判断是使用 process.nextTick 还是 setTimeout 还是 setImmediate，去处理入队列的任务

2.loadMany 方法就是多个 load 方法，只是用了 Promise.all 方法处理

思考：这里关于事件队列 Tick 的思考，我个人理解，举个例子，setTimeout 中回调中使用 setTimeout，那么第一个 setTimeout 执行的时候是第一个 Tick，执行完再将第二个 setTimeout 放入第二个 Tick 中。

参考资料：

https://github.com/graphql/dataloader/blob/master/src/index.js

https://www.jianshu.com/p/fbd1257116b0

https://www.thinbug.com/q/42073880

https://dataloader.js.cool/#/?id=loadkey

http://www.ecma-international.org/ecma-262/6.0/#sec-jobs-and-job-queues


## Vue 路由拦截跳转登录页

背景：实现跳转到各个页面，判断是否登录，假如没有登录，则跳转到登录页

```js
router.beforeEach((to, from, next) => {
  if (to.path === '/login') {
    next()
  } else {
    let token = getCookie('X-BLACKCAT-TOKEN')
    if (!token) {
      next('/login')
    } else {
      next()
    }
  }
})
```


## 一些优质文章

[从0到1，带你尝鲜Vue3.0](https://mp.weixin.qq.com/s/JOxouBFMwGnJCJ1mWYcGBQ)

[「前端进阶」高性能渲染十万条数据(虚拟列表)](https://juejin.im/post/5db684ddf265da4d495c40e5)

[模块的基础操作，导出和导入](https://juejin.im/post/5b2b2d8de51d4558ba1a64e0)



