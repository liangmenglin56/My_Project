#####　同源策略

> 同源策略SOP是Web浏览器的一种安全机制，用于防止不同来源的资源进行不安全的交互。它限制了文档或脚本从一个源加载的资源与另一个源的资源之间的交互，从而保护用户的数据安全

**同源策略的定义**

同源的定义基于三个部分：协议，域名和端口。`只有当两个URL的三个部分完全相同，它们才被认为是同源的`

- 协议：`https:// 或者 http://`
- 域名：`www.example.com 和 sub.example.com`
- 端口：`80`

![img](https://img2023.cnblogs.com/blog/151257/202306/151257-20230605214444046-1605751284.jpg)

同源策略限制了一些Web页面在不同源之间的交互：

- Cookie：同源策略限制了网页只能访问与其同源的Cookie
- DOM：同源策略阻止了不同源的页面对彼此的DOM进行读写操作
- AJAX请求：默认情况下，同源策略阻止网页发起对不同源的AJAX请求



**跨域**

> 虽然同源策略是Web安全的重要基础，但是有些情况下，网站也需要不同源进行交互，尤其是在工程服务化后，不同职责的服务分散在不同的工程中，往往这些工程的域名是不同的，但是一个需求可能对应到多个服务，这时便需要调用不同服务的接口，因此会出现跨域

**跨域方案**

- **CORS跨域资源共享**

  `跨域资源共享，是一种HTTP机制，使用额外的HTTP响应头告诉浏览器让其运行在一个Origin上的web应用被允许访问来自不同源服务器上的指定的资源。当一个资源请求与该资源本身所在的服务器不同的域，协议和端口的资源时，资源会发起一个HTTP跨域请求`

  | 响应头                             | 描述                                                         |
  | ---------------------------------- | ------------------------------------------------------------ |
  | `Access-Control-Allow-Headers`     | 请求头，响应头，预请求（携带 Cookie 情况下不能为 `*`）       |
  | `Access-Control-Allow-Methods`     | 请求头，响应头，预请求（携带 Cookie 情况下不能为 `*`）       |
  | `Access-Control-Allow-Origin`      | 响应头，预请求 / 正常请求（携带 Cookie 情况下不能为 `*`）    |
  | `Access-Control-Allow-Credentials` | 响应头，预请求/正常请求（携带 Cookie 情况下要设置为 `true`） |
  | `Access-Control-Max-Age`           | 响应头，预请求（单位 `s`）                                   |

  `Access-Control-Allow-Origin` 只能阻止浏览器端拿到服务器返回数据，服务端的处理还是会执行，要配合 `token` 鉴权令牌等策略来防范。

- **Node代理跨域**

  `利用Node+ Express + Http-Proxy-Middleware搭建一个Proxy服务器`

  前端实现

  ```javascript
  const xhr = new XMLHttpRequest();
  // 前端开关：浏览器是否读写cookie
  xhr.withCredentials = true;
  
  // 访问http-proxy-middleware代理服务器
  xhr.open('get', 'http://www.domain1.com:3000/login?user=admin', true);
  xhr.send();
  ```

  服务器实现

  ```javascript
  const express = require('express');
  const proxy = require('http-proxy-middleware');
  const app = express();
  
  app.use(
    '/',
    proxy({
      // 代理跨域目标接口
      target: 'http://www.domain2.com:8080',
      changeOrigin: true,
  
      // 修改响应头信息，实现跨域并允许带cookie
      onProxyRes: function (proxyRes, req, res) {
        res.header('Access-Control-Allow-Origin', 'http://www.domain1.com');
        res.header('Access-Control-Allow-Credentials', 'true');
      },
  
      // 修改响应信息中的cookie域名
      cookieDomainRewrite: 'www.domain1.com', // 可以为false，表示不修改
    })
  );
  
  app.listen(3000);
  console.log('Proxy server is listen at port 3000...');
  ```

- **Nginx代理跨域**

  Nginx可实现用于反向代理的异步Web服务器

  ```less
  http {
    include               mime.types;
    default_type          application/octet-stream;
    client_max_body_size  2000M;
    keepalive_timeout     65;
  
    # 虚拟服务器
    server {
      listen       8080;
      server_name  localhost;
  
      # CORS 设置
      # 指定响应资源是否允许与给定的 origin 共享
      add_header Access-Control-Allow-Origin *;
      # 配置是否允许将对请求的响应暴露给页面
      add_header Access-Control-Allow-Credentials 'true';
      # 配置允许跨域的请求头
      add_header Access-Control-Allow-Headers 'Authorization,Content-Type,Accept,Origin,User-Agent,Cache-Control,X-Mx-ReqToken,X-Requested-With';
      # 配置允许跨域的请求方法
      add_header Access-Control-Allow-Methods 'GET,POST,Options';
  
      # 跳过服务端校验，直接返回 200
      if ($request_method = 'OPTIONS') {
        return 200;
      }
  
      location / {
          root        /data/example/dist;
          index       index.html index.htm;
          try_files   $uri /index.html;
          add_header  Cache-Control "private, no-store, no-cache, must-revalidate, proxy-revalidate";
      }
  
      location /api/ {
          proxy_pass  https://xxx.xxx.xxx/req/;
      }
    }
  }
  ```

  

- JSONP

  `动态创建script脚本标签，通过跨域脚本嵌入不受同源策略限制的方法实现请求第三方服务器数据内容，除了适用于script脚本标签，HTML中包含了src和href属性的标签均不受同源策略限制`

  实现步骤：

  1. 动态创建script标签
  2. 标签src属性设置接口地址
  3. 接口参数，必须要带一个自定义函数名
  4. 通过定义函数名去接受后台返回数据

  ```js
  // 动态创建脚本标签
  const script = document.createElement('script');
  
  // 设置接口地址
  script.src = 'http://localhost:8080/api/jsonp?cb=jsonpCallback';
  
  // 插入页面
  document.appendChild(script);
  
  // 通过定义回调函数接收响应数据
  window.jsonpCallback = function (res) {
    // do something with response data
  };
  ```

  后端实现

  ```js
  const Koa = require('koa');
  const fs = require('fs');
  const app = new Koa();
  
  app.use(async (ctx, next) => {
    if (ctx.path === '/api/jsonp') {
      const { cb } = ctx.query;
      ctx.body = `${cb}(${JSON.stringify({ msg })})`;
      return;
    }
  });
  
  app.listen(8080);
  ```

  

- WebSocket

- window.postMessage

- document.domain + iframe

- wondow.location.hash + iframe

- Window.name + iframe