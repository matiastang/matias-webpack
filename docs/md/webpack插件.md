<!--
 * @Author: tangdaoyong
 * @Date: 2021-05-10 15:44:37
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-05-10 16:05:06
 * @Description: webpack插件
-->
# webpack插件

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

`webpack-bundle-analyzer`检查包大小

```js
import WebpackBundleAnalyzer from 'webpack-bundle-analyzer';// 检查包大小

module.exports = {
    ...
    plugins: [
        // 检查包大小
        new WebpackBundleAnalyzer.BundleAnalyzerPlugin()
        ...
    ]
}
```
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