## 什么是 Git

> Git 是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。也是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

## 初始配置

1、配置用户名和电子邮箱

```bash
git config --global user.name "username"
git config --global user.email username@email.com
```

!!! tip
	**--global** 参数代表全局配置，应用于所有项目。如果要指定某个项目的配置，只要去掉 **--global** 选项重新配置即可，新的配置文件保存在当前项目的 .git/config 文件里。

2、查看已有配置信息

```bash
git config --list
```

## 常用命令

- 初始化当前文件夹 `git init`
- 克隆远程仓库 `git clone`
- 查看仓库状态 `git status`
- 添加文件到暂存区 `git add`
- 提交暂存区到本地仓库 `git commit`

## 关系图

``` mermaid
graph LR
  A([工作区]) -->|add| B([暂存区]);
  B -->|commit| C([本地仓库]);
  C -->|push| D[(远程仓库)];
  D -->|fetch| C;
  D -->|clone| A;
  C -->|diff| A;
```

## 提交签名验证

1、生成 SSH 密钥

打开Git Bash

```bash
ssh-keygen -t ed25519 -C "邮箱地址"
```

无特殊需求，无脑回车即可，在  `~/Users/.ssh` 目录下生成私钥 `id_rsa` 和 公钥`id_rsa.pub` 。

2、将密钥告知 Git

```bash
git config --global gpg.format ssh
```

```bash
git config --global user.signingkey ~/.ssh/id_rsa.pub
```

???+ tips 
	也可以直接配置用户目录下的 `.gitconfig` 文件

3、将密钥添加到 GitHub

打开 GitHub，点击头像进入 `Settings` ，左侧选择 `SSH and GPG keys` ，再点击 `New SSH key` 。

需要注意的是，两种 `Key type` 都需要添加，不然提交还是会显示 `Unverfied` 。

???+ quote
	1. Authentication Keys（身份验证密钥）
		- 用于身份验证，允许您与GitHub进行安全通信，例如通过SSH连接。
		- 当您从本地系统与GitHub进行交互时，这是用于验证您身份的密钥。通常，这与访问和推送代码相关。
	2. Signing Keys（签名密钥）
		- 用于对Git提交进行数字签名，以验证提交的真实性和完整性。
		- 这是在Git操作中确保提交的来源和内容未被篡改的一种方式。通过签署提交，您可以确保提交是由特定的私钥持有者创建的。

4、验证

打开Git Bash输入
```bash
ssh -T git@github.com
```

返回以下信息表示成功

```bash
Hi XXXXXX! You've successfully authenticated, but GitHub does not provide shell access.
```

---

