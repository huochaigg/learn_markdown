## 使用同⼀个链接， 如何实现 PC 打开是 web 应用、手机打开是⼀个 H5 应用？

### 方法一：后端重定向（推荐方式）

[nodejs判断和redirect](服务端/nodejs/http和https)

```
app.get('/', (req, res) => {
  const ua = req.headers['user-agent'];
  const isMobile = /mobile|android|iphone|ipad|phone/i.test(ua);

  if (isMobile) {
    res.redirect('/h5'); // 跳转到 H5 应用
  } else {
    res.redirect('/web'); // 跳转到 PC Web 应用
  }
});
```

[Nginx](服务端/nginx/nginx) 示例：

```
server {
  listen 80;
  server_name example.com;

  location / {
    if ($http_user_agent ~* "mobile|android|iphone|ipad|phone") {
      rewrite ^/$ /h5 redirect;
    }
    rewrite ^/$ /web redirect;
  }
}
```

### 方法二：前端 JS 判断跳转：

[navigator](浏览器/网页相关/navigator.userAgent)

```
<script>
  const ua = navigator.userAgent.toLowerCase();
  const isMobile = /mobile|android|iphone|ipad|phone/.test(ua);
  if (isMobile) {
    window.location.href = '/h5';
  } else {
    window.location.href = '/web';
  }
</script>
```

缺点：用户会先加载一部分页面再跳转，体验略差。

### 方法三：响应式设计（单页面适配）

如果你的 H5 和 Web 应用大部分逻辑相似，可以考虑写一个**响应式的单页面应用**，通过媒体查询适配不同尺寸的布局。

例如使用 Vue、React、或者纯 CSS：

[@media](前端/css/@media)

```
@media (max-width: 768px) {
  /* 手机样式 */
}

@media (min-width: 769px) {
  /* PC 样式 */
}

```

|方式|原理|优点|缺点|
|---|---|---|---|
|后端重定向|后端判断 UA|快速准确、体验好|需要后端配合|
|JS 判断跳转|前端判断 UA|实现简单|用户可能看到页面闪一下|
|响应式设计|CSS/JS 适配布局|一套代码适配多端|不适合差异大的场景|
