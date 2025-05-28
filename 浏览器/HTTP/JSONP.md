
## 简单实现

```
function jsonp(url, callbackName) {
  return new Promise((resolve, reject) => {
    const script = document.createElement('script');
    const cbName = callbackName || 'jsonp_cb_' + Date.now();

    window[cbName] = function(data) {
      resolve(data);
      delete window[cbName]; // 清理
      script.remove();       // 删除 script 节点
    };

    script.src = `${url}?callback=${cbName}`;
    script.onerror = reject;
    document.body.appendChild(script);
  });
}

// 使用方式：
jsonp('https://api.example.com/user', 'myCallback')
  .then(data => console.log('获取数据:', data));

```


JSONP 特点：

| 特性         | 说明                                          |
| ---------- | ------------------------------------------- |
| ✅ 优点       | 可用于早期浏览器解决跨域问题（支持 GET）                      |
| ❌ 只能 GET   | 因为 script 标签只能发 GET 请求                      |
| ❌ 无法捕获错误   | 无法像 Ajax 捕获 404、500，只能靠 `onerror`           |
| ❌ 有安全隐患    | 服务端返回的是 JS 执行代码，可能被注入恶意脚本                   |
| 🚫 被现代方案替代 | 如 `CORS`、`postMessage`、`iframe+window.name` |