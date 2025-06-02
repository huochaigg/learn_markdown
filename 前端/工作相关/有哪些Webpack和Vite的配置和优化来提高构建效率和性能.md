前端项目中，**Webpack** 和 **Vite** 各自有一套优化构建效率和产物性能的方式。以下是系统整理的优化手段，分为**构建效率优化**（开发体验）和**产物性能优化**（用户体验）两个部分。

## 一、构建效率优化（开发/构建速度）

### 🌟 [Webpack](webpack/基本功能) 构建效率优化

| 优化项                                   | 说明                                                 |
| ------------------------------------- | -------------------------------------------------- |
| **`cache: true`（持久化缓存）**              | `webpack 5` 默认启用 `filesystem` 缓存，加快二次构建速度。         |
| **`thread-loader`**                   | 多线程处理 loader，如 `babel-loader`。                     |
| **`babel-loader` 缓存**                 | 设置 `cacheDirectory: true` 以缓存 Babel 转译结果。          |
| **DLLPlugin**（老方法）                    | 将第三方依赖提前打包成 DLL（不推荐用于 Webpack 5）。                  |
| **exclude/include 精准匹配**              | 避免 `babel-loader` 等处理 `node_modules`，只处理业务代码。      |
| **`resolve.alias`**                   | 缩短模块查找路径，加快解析速度。                                   |
| **使用 `source-map` 选项合理**              | 开发用 `eval-source-map`，生产用 `hidden-source-map` 或关闭。 |
| **使用 `HardSourceWebpackPlugin`（已弃用）** | Webpack 5 已通过内置缓存取代它。                              |
| **模块热更新（HMR）优化**                      | 精准控制模块热更新区域，避免全量刷新。                                |


### [Vite](vite/vite大纲) 构建效率优化

| 优化项                                   | 说明                                                              |
| ------------------------------------- | --------------------------------------------------------------- |
| **原生 ES 模块 + esbuild**                | `Vite` 开发环境使用 `esbuild` 预构建，极快。                                 |
| **优化 `optimizeDeps.include/exclude`** | 控制 Vite 是否预构建依赖，避免不必要的模块预打包。                                    |
| **使用 `define` 定义常量**                  | `define: { __APP_VERSION__: JSON.stringify('1.0.0') }` 替代运行时代码。 |
| **插件最小化和顺序优化**                        | 减少无用插件，控制插件调用顺序，避免插件冲突和耗时操作。                                    |
| **缓存构建产物**                            | Vite 会自动缓存 `.vite`，注意保留缓存目录。                                    |
| **禁用 sourcemap**                      | `build.sourcemap: false` 可加快构建速度。                               |


## 二、产物性能优化（加载速度/运行性能）

### 🌟 Webpack 产物性能优化

| 优化项                             | 说明                                         |
| ------------------------------- | ------------------------------------------ |
| **代码分割（splitChunks）**           | 提取公共代码和第三方依赖，提升缓存效率。                       |
| **动态导入（import()）**              | 按需加载路由组件等，降低初始包体积。                         |
| **Tree Shaking**                | 移除未使用的模块（确保使用 ES Modules）。                 |
| **使用 TerserPlugin 压缩 JS**       | 默认启用，支持压缩、删除 console/log/debugger。         |
| **MiniCssExtractPlugin 压缩 CSS** | 提取并压缩 CSS。                                 |
| **图片资源优化**                      | 使用 `image-webpack-loader`、WebP 格式、懒加载。     |
| **Gzip/Brotli 压缩**              | 使用 `compression-webpack-plugin`，服务端需支持解压。  |
| **异步组件加载**                      | `React.lazy` / `Vue defineAsyncComponent`。 |


### Vite 产物性能优化

| 优化项                                           | 说明                                                              |
| --------------------------------------------- | --------------------------------------------------------------- |
| **`build.rollupOptions.output.manualChunks`** | 自定义代码拆包策略。                                                      |
| **动态导入优化**                                    | `import()` 结合 `vite-plugin-pages` 实现懒加载。                        |
| **Tree Shaking + ESBuild Minify**             | 默认支持，无需额外配置。                                                    |
| **使用 `vite-plugin-compression`**              | 打包生成 `.gz` 或 `.br` 文件。                                          |
| **图片/资源优化**                                   | 内置 `vite-imagetools` 或自定义 `rollup-plugin-image`，支持 base64、懒加载等。 |
| **预构建依赖合理拆分**                                 | 减少预构建包体积，提高首屏速度。                                                |
| **设置 `base` 正确**                              | 避免生产部署路径错误影响资源加载。                                               |


## 通用建议（Webpack & Vite）

| 优化项                                | 说明                                   |
| ---------------------------------- | ------------------------------------ |
| **使用 CDN 加载第三方库**                  | 避免大库进入打包产物，如 Vue/React。              |
| **使用懒加载和懒注册（组件/路由）**               | 降低首屏压力。                              |
| **字体图标使用 `iconfont` 或 svg sprite** | 减少 HTTP 请求数。                         |
| **避免大型 JSON 文件直接引入**               | 使用动态请求或压缩处理。                         |
| **服务端开启 HTTP 压缩**                  | 支持 Gzip/Brotli。                      |
| **服务端开启缓存头**                       | 利用 `Cache-Control`、`ETag`、`Expires`。 |


## 推荐插件合集

### Webpack 插件

- `terser-webpack-plugin`
    
- `compression-webpack-plugin`
    
- `webpack-bundle-analyzer`
    
- `image-webpack-loader`
    
- `MiniCssExtractPlugin`
    
- `IgnorePlugin`
    

### Vite 插件

- `vite-plugin-compression`
    
- `vite-plugin-imagemin`
    
- `vite-plugin-pages`
    
- `vite-plugin-pwa`
    
- `vite-plugin-inspect`（分析插件执行顺序）