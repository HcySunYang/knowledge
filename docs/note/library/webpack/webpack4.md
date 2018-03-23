## webpack4.0+ 特性总结

#### entry

* 默认值：`'./src'`

#### output

* 默认值：`'./dist'`
* 占位符：`[hash]`、`[chunkhash]`、`[name]`、`[id]`、`[query]`

#### mode

* `mode` 选项有两个可选值：`'development'`、`'production'` 以及隐藏值 `none`。
* 默认值：`'production`
* `development mode`
  * 默认开启：
    * `optimization.namedModules`（之前的 `webpack.NamedModulesPlugin`）
    * 设置 `process.env.NODE_ENV` 的值为 `development`
    * `output.pathinfo`
  * 默认关闭：
    * `optimization.minimize`
* `production mode`
  * 默认开启：
    * `optimization.noEmitOnErrors`（之前的 `webpack.NoEmitOnErrorsPlugin`）
    * `optimization.concatenateModules`（之前的 `webpack.optimize.ModuleConcatenationPlugin`）
    * `UglifyJsPlugin`
    * 设置 `process.env.NODE_ENV` 的值为 `production`
  * 默认关闭：
    * `in-memory caching`
* `none mode`
  * 不启用任何模式

#### 支持的模块类型

* `webpack2.0+` 的版本能够处理 ES2015 模块（即：`import` 语句） 
* 支持 `CommonJS`（即：`require()` 语句）
* 支持 `AMD` （即：`defined` 和 `require`）
* 支持 `css/less/sass` 的 `@import` 语句
* 支持引用图片的语句（即：`url(...)` 和 `<img src="..." />`）

* CoffeeScript
* TypeScript
* ESNext (Babel)
* Sass
* Less
* Stylus

#### 模块解析规则

![](http://7xlolm.com1.z0.glb.clouddn.com/2018-03-23-094233.jpg)

#### 主要变更

* 必须选择一个模式(`mode`)，`production` 或 `development` 或者隐藏值 `none`。
* `CommonsChunkPlugin` 被移除，使用 `optimization.splitChunks` 和 `optimization.runtimeChunk` 代替。
* 使用 `optimization.namedModules` 替代 `webpack.NamedModulesPlugin`
* 使用 `optimization.noEmitOnErrors` 替代 `webpack.NoEmitOnErrorsPlugin`
* 使用 `optimization.concatenateModules` 替代 `webpack.optimize.ModuleConcatenationPlugin`
* `import()` 语句总是返回命名空间对象，`CommonJS` 模块被包装为默认导出（`default export`）
* `webpack` 原生支持使用 `ES Module` 导入 `JSON`。

#### 新增特性

* `webpack` 原生支持如下模块类型：

#### 移除的特性

* 移除 `module.loaders`
* 移除 `loaderContext.options`
* 移除 `Compilation.notCacheable` flag
* 移除 `NoErrorsPlugin`
* 移除 `Dependency.isEqualResource`
* 移除 `NewWatchingPlugin`
* 移除 `CommonsChunkPlugin`
