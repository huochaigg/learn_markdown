
`navigator.sendBeacon()` 是浏览器提供的一种**异步、非阻塞、可靠的少量数据发送方式**，常用于在页面卸载（如跳转或关闭）时向服务器发送数据，比如用于上报用户行为或日志数据。

```
navigator.sendBeacon(url, data);
```

例如：

```
window.addEventListener("unload", function () {
  const data = JSON.stringify({ event: "page_unload", timestamp: Date.now() });
  navigator.sendBeacon("/log", data);
});
```


