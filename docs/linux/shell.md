## Shell

Shell，中文释义为“壳”，像是包裹Linux系统内核的外壳，用户只需要通过触摸这层外壳来操作系统，是用户使用Linux的桥梁，将用户操作转译成机器能处理的0和1。

!!!tip
	此处shell指一种应用程序，提供用户与系统内核的交互界面。同时shell是一种脚本语言，也是一种程序设计语言。

Shell的种类有很多：

- Power Shell（Windows）
- Bourne Shell（/usr/bin/sh或/bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- ……

!!!tip
	bash通常是大多数Linux默认的shell，可通过 `echo $SHELL` 命令来查看。

Shell脚本的三种执行方式:

- `./name.sh`

先按照文件中指定的解析器解析，如果指定的解析器不存在，使用系统默认的解析器。

- `bash name.sh`

先用bash解析器解析，如果bash不存在，则使用默认解析器。

- `. name.sh`

直接使用默认解析器解析，忽略文件中指定的解析器，但是第一行还是要写。