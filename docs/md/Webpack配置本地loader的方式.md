<!--
 * @Author: tangdaoyong
 * @Date: 2021-05-18 12:04:43
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-05-18 12:06:56
 * @Description: webpack配置本地loader的方法
-->
# webpack配置本地loader的方法

[webpack配置本地loader的四种方法](https://champyin.com/2020/01/04/webpack%E9%85%8D%E7%BD%AE%E6%9C%AC%E5%9C%B0loader%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E6%B3%95/)

通常我们配置 loader 只需直接使用 loader 的名字，不用关心 loader 的路径。那是因为通过 npm 或者 yarn 安装的 loader 都会安装在 node_modules 目录下，而 webpack 默认所有第三方模块都会去 node_modules 里找。

当我们要使用本地 loader (例如测试自己开发的loader)，而这些模块不在 node_modules 里的时候，就需要告诉 webpack 存放 loader 的位置。

在 webpack4.0 里，一共有四种方法配置本地loader：

1. 在配置 rules 的时候直接指定 loader 的绝对路径
module.exports = {
  // xxx
  module: {
    rules: [
      {
        test: /\.js$/,
        // 在这里配置绝对路径
       use: path.resolve(__dirname, 'loaders/myLoader.js')
      }
    ]
  }
}
2. 或者在 resolveLoader 里配置 alias 别名
module.exports = {
  // xxx
  resolveLoader: {
  // 配置 resolveLoader.alias
    alias: {
      myLoader: path.resolve(__dirname, 'loaders/myLoader.js')
    }
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'myLoader'
      }
    ]
  }
}
3. 还可以在 resolveLoader 里配置 modules 属性
将放置 loader 的目录告诉 webpack。当 webpack 在默认目录下找不到指定 loader 时，会自动去这个目录查找。resolveLoader.modules 是个数组，可以配置多个路径。


module.exports = {
  // xxx
  resolveLoader: {
  // 配置 resolveLoader.modules
    modules: ['node_modules', path.resolve(__dirname, 'loaders']
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'myLoader'
      }
    ]
  }
}
4. 还可以使用 npm link
把 loader 从当前项目抽离出来，构建独立工程。
在 loader 工程目录下执行 npm link;
回到原项目目录，执行 npm link xxx (xxx为loader的名称)。
最后，在原项目使用时，直接使用名称即可 (跟 npm install 的 loader 一样使用)。
如果对 npm link 原理感兴趣，可以看一看这篇文章 [npm link详解](https://champyin.com/2019/08/27/npm-link%E8%AF%A6%E8%A7%A3/)。