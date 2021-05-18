<!--
 * @Author: tangdaoyong
 * @Date: 2021-05-10 15:44:37
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-05-18 11:01:00
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
            _: 'lodash',
            $: 'jquery',
            api: 'api'
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
