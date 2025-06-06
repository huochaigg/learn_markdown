```
function ajax(options) {
  const xhr = new XMLHttpRequest();
  const method = options.method ? options.method.toUpperCase() : 'GET';
  let url = options.url;
  const isGet = method === 'GET';

  // 参数序列化
  const params = options.data ? Object.entries(options.data).map(
    ([key, val]) => `${encodeURIComponent(key)}=${encodeURIComponent(val)}`
  ).join('&') : '';

  if (isGet && params) {
    url += (url.includes('?') ? '&' : '?') + params;
  }

  xhr.open(method, url, true);

  // 设置请求头
  if (!isGet) {
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
  }
  if (options.headers) {
    for (const key in options.headers) {
      xhr.setRequestHeader(key, options.headers[key]);
    }
  }

  // 超时设置（可选）
  if (options.timeout) {
    xhr.timeout = options.timeout;
  }

  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) {
      const res = xhr.responseText;
      if (xhr.status >= 200 && xhr.status < 300) {
        options.success && options.success(JSON.parse(res));
      } else {
        options.error && options.error(xhr.status, xhr.statusText);
      }
    }
  };

  xhr.ontimeout = function () {
    options.error && options.error('timeout', '请求超时');
  };

  xhr.onerror = function () {
    options.error && options.error('error', '网络错误');
  };

  xhr.send(isGet ? null : params);
}

```