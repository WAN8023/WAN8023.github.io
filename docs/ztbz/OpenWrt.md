### 防火墙

#### # 删除在OpenWrt防火墙中的自定义规则会导致局域网访问缓慢、卡顿

  ```bash
  iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 53
  iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 53
  ```

#### # 局域网访问

<img src="./assets/OpenWrt/image-20230531110658228.png" alt="image" style="zoom: 67%;" />

默认设置下，**Laptop** 可以访问 **HUAIWEI** 和 **主路由**，但无法访问 **OpenWrt**，更改防火墙中设置WAN口入站数据为接受后可以访问。

### SSH

#### # TTYD终端拒绝连接

第一种：

编辑TTYD的配置文件

`vim /etc/init.d/ttyd`

注释

`#${interface:+-i $interface}`

重启TTYD

`/etc/init.d/ttyd restart`

第二种：

编辑 `/etc/config/ttyd`

更改 `option interface '@lan'`中的`@lan`为合适接口

### iStore

#### # 手动安装失败

  终端下查看编译的架构版本`cat /etc/openwrt_release`

  ![image](./assets/OpenWrt/image-20230604161558228.png)

  更改OPKG配置

```bash
dest root /
dest ram /tmp
lists_dir ext /var/opkg-lists
option overlay_root /overlay
# option check_signature

arch all 100
arch aarch64_generic 200
arch aarch64_cortex-a53 300
```
