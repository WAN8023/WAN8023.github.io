## OpenResty

白嫖了半年的阿里云服务器，想着随便搭一点儿东西玩一玩，结果域名解析到国内的服务器需要备案，时间太久太麻烦，遂放弃。于是想着用不同路径区分服务，反向代理到服务器上的不同服务。

```http
http://ip/app1  =>  127.0.0.1:6185		服务1
http://ip/app2  =>  127.0.0.1:6266		服务2
http://ip/app3  =>  127.0.0.1:6899		服务3
```

先用 `1Panel` 面板上的 `OpenRestry` 尝试。创建后访问显示一片空白，查看日志发现资源都是 404。

推测 `Nginx` 成功把请求发给了 6185 端口的 `AstrBot`，也成功返回了基础的 HTML 文件（日志里没有 `/astrbot/` 的 404 报错）。

但是依照 HTML 代码写死了静态资源的位置，比如 `<script src="/js/index.js"></script>`，浏览器看到 `/js/...`，认为它在网站的根目录下，于是向服务器发起请求：`https://ip/js/index.js`，但是 Nginx 只配置了`/astrbot/`的转发，无法找到`/js/`、`/css/`的请求，返回 `404 Not Found`。

按照 `gemini` 给出的解决办法，可以在 `Nginx` 中为静态资源也设置转发，在 `location ^~ /astrbot/ { ... }` 下同级添加一个 `location` 块。

```nginx
# 原本的配置（保持不变）
location ^~ /astrbot/ {
proxy_pass http://127.0.0.1:6185/; 
proxy_set_header Host $host; 
# ... 其他 headers
}

# ====== 新增下面这段“补丁”配置 ======
# 拦截所有对特定前端资源目录的请求，并转发给 AstrBot
location ~* ^/(assets|css|js|img|manifest\.json) {
proxy_pass http://127.0.0.1:6185; # 注意这里最后不要加斜杠
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

!!! failure
	这样确实能解决服务白屏的问题，但是但它会独占这个 IP 根目录下的这些文件夹。如果在这个 `Nginx` 下面还跑了其他的网页（比如一个博客），且那个博客也需要访问根目录的 `/css/`，流量就会全被转发到 `AstrBot` 这里，导致其他服务崩溃，意味着这台服务器只跑 `AstrBot`。
	并且，即使网页成功显示，依旧会登陆报错 `AxiosError: Request failed with status code 404`。类似的，还要继续添加 `/api/` 的路径转发。

总的来说，这种方法麻烦，违背了初衷，并且存在很多问题，放弃！！！
