## 什么是 Zsh ？

> Z shell（Zsh）是一款可用作交互式登录的shell及脚本编写的命令解释器。Zsh对Bourne shell（sh）做出了大量改进，同时加入了Bash、ksh及tcsh的某些功能。
>
> 自2019年起，macOS的默认Shell已从Bash改为Zsh。

- 安装

```bash
sudo apt install zsh
```

- 更改默认shell

```bash
sudo chsh -s /bin/zsh
```

## 美化 zsh

<div class="annotate" markdown>

- 安装 oh my zsh (1)

</div>

1. oh my zsh 是一个开源框架，用于管理 Zsh 配置。


=== "wget Install"
	```bash
	sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
	```

=== "curl Install"
	```bash
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
	```

<div class="annotate" markdown>

- 安装 Powerlevel10k (1)

</div>

1. PowerLevel10k 是 Zsh 的一个主题。它强调速度，灵活性和开箱即用的体验。

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
```

```bash
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```

- 重新连接，按需配置
- 使用 `p10k configure` 可重新配置