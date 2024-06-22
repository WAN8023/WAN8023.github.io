## 基本配置

0.需求

在服务器上部署很多应用后，访问需要在浏览器中输入 `服务器公网IP:端口号` 的形式，可通过反向代理实现访问不同的二级域名来访问不同的应用。这里使用管理 Nginx 代理服务器的开源工具项目——[**Nginx Proxy Manager**](https://github.com/NginxProxyManager/nginx-proxy-manager)。

1.域名解析

在 Cloudflare 上将域名解析到服务器公网 IP，`*.wan8023.top` 为泛域名解析，即所有二级域名都将解析到这个服务器 IP。

<img src="./assets/Nginx Proxy Manager/image-20240602112013.png" alt="image-20240602112013" style="zoom: 50%;" />

2.Docker 部署 NPM

`docker-compose up -d`，NPM 使用 80，443，81 这三个端口，80 和 443 分别是 nginx 的 http 代理端口和 https 代理端口，81 为 NPM 面板服务端口。

3.反向代理

以 NPM 本身 web 面板为例，将二级域名 `nginx.wan8023.top` 指向该服务。IP 栏填写公网 IP，则需要开放对应端口 81。

<img src="./assets/Nginx Proxy Manager/image-20240602113834.png" alt="image-20240602113834" style="zoom:50%;" />

<img src="./assets/Nginx Proxy Manager/image-20240602120721.png" alt="image-20240602120721" style="zoom:50%;" />

当 NPM 服务与需要反代的服务在同一服务器时，IP 栏可以填写为 `172.17.0.1`，此时只需要开放 Nginx 服务需要的 80 和 443 端口即可，这样可以避免使用 `IP:端口号` 仍可以访问对应服务。

4.总结

以 NPM 的 web 服务为例，客户端使用浏览器访问二级域名 `nginx.wan8023.top`，浏览器将域名解析为 `34.92.43.45:80`，Nginx 服务监听 80 端口，根据二级域名不同，将其指向服务器中的不同服务。

<img src="./assets/Nginx Proxy Manager/image-20240602121845.png" alt="image-20240602121845" style="zoom:50%;" />

## NPM 重置密码

1.登录到容器内部

`docker exec -it nginxproxymanager /bin/sh`

2.连接到数据库

`sqlite3 /data/database.sqlite`

!!!tip
    如果命令未找到，运行  `apt update && apt install sqlite3 -y` 安装工具

3.重置密码为 `changeme`

```bash
UPDATE auth
SET secret = '$2b$13$X.7n1og.RCdKg/p/ZRUIj.gGlFPmoeSK29DTcF58tBzS6d1fZNJNu'
WHERE id = 1;
```

4.退出数据库
`.quit`

## NPM 反代 X-UI

0.通过 NPM 反向代理使得 x-ui 面板的域名与代理伪装 host 的域名不同

<img src="./assets/Nginx Proxy Manager/image-20240602163534.png" alt="image-20240602163534" style="zoom: 67%;" />

1.x-ui 面板新建入站

<img src="./assets/Nginx Proxy Manager/image-20240602163857.png" alt="image-20240602163857" style="zoom:50%;" />

2.特殊配置

2.1 导入工具并不能直接使用，需要手动配置

<img src="./assets/Nginx Proxy Manager/image-20240602164352.png" alt="image-20240602164352" style="zoom:50%;" />

2.2 编辑进行设置

端口改为 443，打开 TLS

<img src="./assets/Nginx Proxy Manager/image-20240602165954.png" alt="image-20240602165954" style="zoom:50%;" />

## 证书介绍

|    文件名     |                             作用                             |
| :-----------: | :----------------------------------------------------------: |
|   cert.pem    |                         服务器端证书                         |
|   chain.pem   | 浏览器需要的所有证书但不包括服务端证书，比如根证书和中间证书 |
| fullchain.pem |               包括了cert.pem和chain.pem的内容                |
|  privkey.pem  |                          证书的私钥                          |

