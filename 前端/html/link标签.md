link标签是 HTML 的“外部资源引入器”，通常放在 `<head>` 标签中

### 引入样式表

```
<link rel="stylesheet" href="styles.css">
```

必须放在 `<head>` 中，保证 CSS 提前加载。

- `rel="stylesheet"`：表示链接的是样式表。
- `href`：指定样式文件路径。


### 定义网页图标

```
<link rel="icon" href="/favicon.ico" type="image/x-icon">
```

浏览器地址栏、标签页、收藏夹等显示网站图标。

**常见类型**：`image/x-icon`、`image/png` 等

### 预加载资源（优化性能）

```
<link rel="preload" href="/font.woff2" as="font" type="font/woff2" crossorigin="anonymous">
```

告诉浏览器提前加载某些资源，提升性能。

**常见 `rel` 值**：
- `preload`：预加载资源但不立即使用。
- `prefetch`：浏览器空闲时预取资源，供将来使用。
- `dns-prefetch`：提前解析某个域名。
- `preconnect`：提前建立到第三方服务器的连接（比如 CDN）。
- `modulepreload`：预加载模块脚本（如 ES Modules）。


### 引入网页的关联信息（SEO、社交、PWA）

```
<link rel="canonical" href="https://example.com/page">
<link rel="manifest" href="/manifest.json">
<link rel="alternate" hreflang="en" href="/en/page">
```

- **canonical**：声明页面的“权威地址”，有助于 SEO。
- **manifest**：引入 PWA 的 manifest 文件。
- **alternate**：指明不同语言版本的地址。

### RSS/ATOM 订阅源

```
<link rel="alternate" type="application/rss+xml" title="RSS" href="/feed.xml">
```

为博客、新闻等提供订阅源。


### 引入外部字体（配合 `<style>` 或独立引入）

```
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Roboto&display=swap">
```

引入 Google Fonts 或其他字体服务。


### 模块类型的脚本预加载（支持 ES Modules）

```
<link rel="modulepreload" href="/main.mjs">
```

ES Module 脚本预加载，用于优化模块加载速度。


## 常见的 `rel` 类型一览

|rel 值|作用说明|
|---|---|
|stylesheet|引入 CSS|
|icon|网页图标（favicon）|
|preload|提前加载资源（提升首屏速度）|
|prefetch|未来可能用到的资源预取|
|dns-prefetch|DNS 提前解析|
|preconnect|提前建立连接（如 CDN）|
|canonical|声明权威地址（SEO）|
|manifest|引入 PWA 配置|
|alternate|多语言/订阅源等备用链接|
|modulepreload|预加载模块脚本（ES Module）|