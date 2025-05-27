## 基本语法

```
<iframe src="https://example.com" width="600" height="400"></iframe>
```

常用属性：

| 属性                | 含义                                                       |
| ----------------- | -------------------------------------------------------- |
| `src`             | 要加载的网页地址                                                 |
| `width`           | iframe 的宽度（可以是百分比或像素）                                    |
| `height`          | iframe 的高度                                               |
| `name`            | iframe 的名称，可用于 `window.frames["name"]` 或 `target="name"` |
| `sandbox`         | 限制 iframe 权限的沙箱（安全相关）                                    |
| `allow`           | 允许的功能（如摄像头、全屏等）                                          |
| `loading`         | 延迟加载策略（如 `lazy`）                                         |
| `referrerpolicy`  | 控制 referrer 的发送行为                                        |
| `allowfullscreen` | 是否允许全屏显示                                                 |

## iframe 的用途

- **嵌入第三方页面**（如广告、地图、视频）
    
- **页面内嵌架构**：比如多个子页面用 iframe 切换展示
    
- **内容隔离**：利用 iframe 将不可信内容与主页面隔离
    
- **前端微服务（微前端）**中的一个实现方案
    
- **跨域通信**（通过 `postMessage` 实现）

## 安全沙箱 sandbox 属性

`sandbox` 属性开启时，iframe 会被限制很多能力。可以指定白名单功能：

```
<iframe src="..." sandbox="allow-scripts allow-forms"></iframe>
```

| sandbox 值              | 作用                           |
| ---------------------- | ---------------------------- |
| `allow-same-origin`    | 保留同源策略，否则 iframe 内即使同域也被视为跨域 |
| `allow-scripts`        | 允许执行脚本（默认不允许）                |
| `allow-forms`          | 允许提交表单                       |
| `allow-popups`         | 允许打开弹窗                       |
| `allow-modals`         | 允许使用模态对话框                    |
| `allow-top-navigation` | 允许跳转主窗口                      |
| `allow-presentation`   | 允许进入全屏演示                     |

## 同源策略

如果 iframe 加载的是 **非同源页面**，则不能直接访问其 DOM 或变量。即：

```
iframe.contentWindow.document  // 跨域时报错
```

使用 `postMessage` 跨域通信

```
// 主页向iframe发送消息
const iframe = document.querySelector('iframe');
iframe.contentWindow.postMessage({ msg: 'hello' }, 'https://example.com');

// iframe监听消息
window.addEventListener('message', (event) => {
  if (event.origin === 'https://your-origin.com') {
    console.log(event.data);
  }
});
```

## 其他设置

去除边框

```
<iframe style="border: none;"></iframe>
```

**全屏控制**：

```
<iframe allowfullscreen></iframe>
```

自适应高度：

```
同源可用 `iframe.contentWindow.document.body.scrollHeight` 
跨域需用 `postMessage` 通知主页面高度调整
```

## SEO

- iframe 内的内容 **不被主页面继承 SEO 权重**
    
- 会增加网络请求和 DOM 开销，不建议滥用
    
- 可加 `loading="lazy"` 提升首屏性能

## 安全性
- 可通过 CSP 限制 iframe 加载来源：
Content-Security-Policy: frame-src https://trusteddomain.com
- 使用 `sandbox` 提升安全性，尤其是加载第三方不可信内容时
    
- 不信任的 iframe **禁止 allow-scripts、allow-same-origin** 同时使用，避免 XSS+同源攻击


## 用动态创建iframe来触发h5和App交互

在旧版本 WebView 或不支持 `window.webkit.messageHandlers/JSBridge` 的环境下，通常会用此方法来和app交互

```
const iframe = document.createElement('iframe');
iframe.style.display = 'none';
iframe.src = 'myapp://doSomething?param=value';
document.body.appendChild(iframe);

// 延迟清理，避免积累
setTimeout(() => {
  document.body.removeChild(iframe);
}, 0);
```
自定义协议（如 `myapp://`）会被 App 拦截处理，而不会实际去网络请求。

### IOS注意：

- iOS WKWebView 中，**动态 iframe 不总是触发 URL scheme**
- 推荐使用 `window.location = 'myapp://...'` 或通过 `window.webkit.messageHandlers`
### Android 中的坑：

有些旧设备需加延迟：iframe 加载太快未被原生捕获

```
setTimeout(() => {
  document.body.appendChild(iframe);
}, 50);
```

### 多次调用避免累积
iframe 不要反复创建未销毁；建议统一管理一个隐藏容器并复用


### 替代方案

现代 Hybrid App 更推荐：

- ✅ JSBridge 通信（如 iOS 的 `window.webkit.messageHandlers`，Android 的 `addJavascriptInterface`）
    
- ✅ WebViewJavascriptBridge（第三方桥接库）
    
- ✅ postMessage（通过 iframe、WebView 消息通道）

iframe hack 方法适用于旧系统兼容，**新项目推荐逐步迁移**到更安全、标准的通信方式。


### *鸿蒙系统如何实现联调*