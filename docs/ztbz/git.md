## 提交签名验证

- 生成 SSH 密钥

打开Git Bash

```bash
ssh-keygen -t ed25519 -C "邮箱地址"
```

无特殊需求，无脑回车即可，在  `~/Users/.ssh` 目录下生成私钥 `id_rsa` 和 公钥`id_rsa.pub` 。

- 将密钥告知 Git

```bash
git config --global gpg.format ssh
```

```bash
git config --global user.signingkey ~/.ssh/id_rsa.pub
```

???+ tips 
	也可以直接配置用户目录下的 `.gitconfig` 文件

- 将密钥添加到 GitHub

打开 GitHub，点击头像进入 `Settings` ，左侧选择 `SSH and GPG keys` ，再点击 `New SSH key` 。

需要注意的是，两种 `Key type` 都需要添加，不然提交还是会显示 `Unverfied` 。

???+ quote
	1. Authentication Keys（身份验证密钥）
		- 用于身份验证，允许您与GitHub进行安全通信，例如通过SSH连接。
		- 当您从本地系统与GitHub进行交互时，这是用于验证您身份的密钥。通常，这与访问和推送代码相关。
	2. Signing Keys（签名密钥）
		- 用于对Git提交进行数字签名，以验证提交的真实性和完整性。
		- 这是在Git操作中确保提交的来源和内容未被篡改的一种方式。通过签署提交，您可以确保提交是由特定的私钥持有者创建的。

- 验证

打开Git Bash输入
```bash
ssh -T git@github.com
```

返回以下信息表示成功

```bash
Hi XXXXXX! You've successfully authenticated, but GitHub does not provide shell access.
```

---

