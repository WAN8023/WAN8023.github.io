## 面板搭建

在谷歌云新建一台实例，E2 的性能就足够了

<img src="./assets/3X-UI/image-20260106110908849.png" alt="image-20260106110908849" style="zoom:50%;" />

<img src="./assets/3X-UI/image-20260106111022215.png" alt="image-20260106111022215" style="zoom:50%;" />

建议关闭 `Ops Agent`，否则可能会导致网卡数据异常，因为会上传监控数据给 Google，但权限不足，会反复上传

<img src="./assets/3X-UI/image-20260106111137176.png" alt="image-20260106111137176" style="zoom:50%;" />

查看实例 ip 并复制

<img src="./assets/3X-UI/image-20260106111655923.png" alt="image-20260106111655923" style="zoom:50%;" />

在 [CloudFlare](https://dash.cloudflare.com) 将域名解析到服务器的 ip 地址，关闭小黄云

<img src="./assets/3X-UI/image-20260106112444604.png" alt="image-20260106112444604" style="zoom:50%;" />

SSH 连接到实例，记得防火墙打开 22 端口，谷歌默认入站端口全部关闭，出站全部打开

依次输入命令


```shell
sudo -i
sudo timedatectl set-timezone Asia/Shanghai
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

选择自定义端口为 443，输入刚才 DNS 解析到此服务器 ip 的域名

<img src="./assets/3X-UI/image-20260106113601968.png" alt="image-20260106113601968" style="zoom: 80%;" />

访问面板

![image-20260106113704777](./assets/3X-UI/image-20260106113704777.png)

配置面板 https

<img src="./assets/3X-UI/image-20260106114159314.png" alt="image-20260106114159314" style="zoom: 50%;" />

新建入站

![image-20260106114931141](./assets/3X-UI/image-20260106114931141.png)

<img src="./assets/3X-UI/image-20260106120644561.png" alt="image-20260106120644561" style="zoom:50%;" />
