<!--
 * @Author: tangdaoyong
 * @Date: 2021-05-10 15:44:37
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-05-24 15:48:11
 * @Description: webpack插件
-->
# webpack插件

## webpack内置插件

## ProvidePlugin

`webpack`配置`ProvidePlugin`后，在使用时将不再需要`import`和`require`进行引入，直接使用即可。

```js
{
    ...
    plugins: [
        // 配置全局直接使用，不需要导入
        new webpack.ProvidePlugin({
            _: 'lodash',// 三方库
            _concat: ['lodash', 'concat'], // 三方库中的某个工具函数可以全局使用
            message: ['antd', 'message'],
            $: 'jquery',
            api: 'api'// 自定义文件
        })
        ...
    ],
    resolve: {
        extensions: ['.js', '.vue', '.json'],
        alias: {
            'api': resolve('src/api/index.js'),
        }
    },
}
```

这样的话，使用别名访问`src/api/index.js`，然后使用`ProvidePlugin`插件导入别名，这样在使用时就不用导入了。

## remove-unused-files-webpack-plugin

[remove-unused-files-webpack-plugin](https://www.npmjs.com/package/remove-unused-files-webpack-plugin)

```js
import RemoveUnusedFilesWebpackPlugin from 'remove-unused-files-webpack-plugin';// 清除无用文件

module.exports = {
    ...
    plugins: [
        // 清除无用文件
        new RemoveUnusedFilesWebpackPlugin({
            patterns: ['src/**'],// 目录
            remove: true// 提示错误
        })
        ...
    ]
}
```

## unused-files-webpack-plugin

[unused-files-webpack-plugin](https://www.npmjs.com/package/unused-files-webpack-plugin)

```js
import UnusedFilesWebpackPlugin from 'unused-files-webpack-plugin';// 清除无用文件

module.exports = {
    ...
    plugins: [
        // 清除无用文件
        new UnusedFilesWebpackPlugin({
            patterns: ['src/**'],// 目录
            failOnUnused: true// 提示错误
        })
        ...
    ]
}
```

## webpack-bundle-analyzer
## clean-webpack-plugin

`clean-webpack-plugin`清除编译目录。

```js
import { CleanWebpackPlugin } from 'clean-webpack-plugin';// 清除编译目录

module.exports = {
    ...
    plugins: [
        // 清除编译目录
        new CleanWebpackPlugin()
        ...
    ]
}
```

## copy-webpack-plugin

[copy-webpack-plugin](https://www.npmjs.com/package/copy-webpack-plugin)
将`文件`或`目录`直接拷贝到`构建目录`下。

```js
const CopyWebpackPlugin = require("copy-webpack-plugin");

{
    ...
    plugins: [
        // 清除编译目录
        new CopyWebpackPlugin([
            {
                from: __dirname + '/node_modules/ahc-ui/dist/assets/data/',
                to: `${ASSETS_PATH}/data/`,
                toType: 'dir'
            },
            {
                from: __dirname + '/src/images',
                to: `${ASSETS_PATH}/images/`,
                toType: 'dir'
            }
        ])
        ...
    ]
}

plugins: [
        // check package size
        // new WebpackBundleAnalyzer.BundleAnalyzerPlugin(),
        // 清除无用文件
        new UnusedFilesWebpackPlugin({
            patterns: ['src/**']
        }),
        // 清除编译目录
        new CleanWebpackPlugin(),
        // 配置全局变量
        new webpack.DefinePlugin({
            ...define(globals),
            'process.env.NODE_ENV': JSON.stringify('development')
        })
    ]

plugins: [
            ,
            new MiniCssExtractPlugin({
                filename: `${ASSETS_PATH}/css/[name].[contenthash].css`,
                chunkFilename: `${ASSETS_PATH}/css/[name].[contenthash].css`   // chunk css file
            }),
            // index.html 模板插件
            new HtmlWebpackPlugin({                             
                faviconPath: `${ASSETS_PATH}/images/favicon.ico`,
                filename: 'index.html',
                template: './src/template.ejs'
            }),
            // style规范校验
            new StyleLintPlugin({
                context: 'src',
                files: '**/*.(c|sc|sa|le)ss',
                fix: true,
                cache: true
            }),
            new MomentLocalesPlugin({       // moment库插件
                localesToKeep: ['zh-cn']
            }),
            // 文件大小写检测
            new CaseSensitivePathsPlugin()                      
        ]
```

防止重复CommonsChunkPlugin（code-split）
* 在webpack配置文件的plugins配置这一项（不过这个方式，4.0已经过时了）

new webpack.optimize.CommonsChunkPlugin({
    name: 'common'
})
* webpack 4.0新的防重复分割方式,配置optimization。

optimization: {
    splitChunks: {
        cacheGroups: {
            commonjs: {
                chunks: 'initial',
                minChunks: 2,
                maxInitialRequests: 5,
                minSize: 0
            },
            vendor: {
                test: /node_modules/,
                chunks: 'initial',
                name: 'vendor',
                priority: 10,
                enforce: true
            }
        }
    },
    runtimeChunk: true
},

Vue中懒加载
推荐一篇文章：[Lazy Loading in Vue using Webpack's Code Splitting](https://link.zhihu.com/?target=https%3A//alexjover.com/blog/lazy-load-in-vue-using-webpack-s-code-splitting/)

Lazy load in Vue components
Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。

Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
也可以在工厂函数中返回一个 Promise。这是在普通项目中，最常用的语法。

Vue.component(
  'async-webpack-example',
  // 这个 `import` 函数会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
Lazy load in Vue router
最常用的技术：Vue-router的路由懒加载

写法：

const Foo = () => import('./Foo.vue')

const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
把组件按组分块的写法：

const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
Lazy load a Vuex module