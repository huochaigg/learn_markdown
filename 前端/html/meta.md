
`<meta>` 标签是 HTML `<head>` 部分中使用的重要标签之一，用于提供页面的**元数据**（metadata），即不会直接显示在页面上的信息。这些信息主要用于浏览器、搜索引擎、社交媒体等处理网页内容时的参考。

### 字符编码

```
<meta charset="UTF-8">
```


### 视口设置（响应式设计）

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### **SEO 相关**

```
<!-- 页面描述（搜索结果摘要） -->
<meta name="description" content="这是一个介绍HTML meta标签的网页">

<!-- 搜索引擎已较少使用 -->
<meta name="keywords" content="HTML, meta, SEO, 页面优化">

<!-- 页面作者 -->
<meta name="author" content="Your Name">

```

```
<!------------------ 其他 --------------------->
<!-- 禁止搜索引擎索引，用于控制爬虫行为，如禁止收录页面。 -->
<meta name="robots" content="noindex, nofollow">

<!-- 自动刷新和跳转。表示 5 秒后跳转到指定 URL。 -->
<meta http-equiv="refresh" content="5;url=https://example.com">

<!-- 兼容性设置（IE） -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">

<!-- 社交媒体预览（Open Graph, Twitter Cards 等） -->
<!-- Open Graph（Facebook 等平台） -->
<meta property="og:title" content="网页标题">
<meta property="og:description" content="网页描述">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="网页标题">
<meta name="twitter:description" content="网页描述">
<meta name="twitter:image" content="https://example.com/image.jpg">

```

- `<meta>` 标签顺序推荐：`charset` 最先，其次是 `viewport`，再是其他标签。
- 一些 SEO 工具有助于自动生成适合你页面的 meta 标签（如 Yoast SEO, metatags.io）。