## Linux

linux系统由linux内核、shell、文件系统和第三方应用软件组成。

### shell命令的基本格式 ###

    cmd [options] [arguments]  

options 称为选项，arguments 称为参数.  
选项和参数都作为Shell命令执行时的输入，它们之间用空格分隔开。  

### tips ###

- Linux是区分大小写的
- 一般来说，单字符选项前使用一个减号。单词选项前使用两个减号

- 内置命令：出于效率的考虑，将一些常用命令的解释程序构造在Shell内部。
- 外置命令：存放在/bin、/sbin目录下的命令
- 实用程序：存放在/usr/bin、/usr/sbin、/usr/share、/usr/local/bin等目录下的实用程序
- 用户程序：用户程序经过编译生成可执行文件后，可作为Shell命令运行
- Shell脚本：由Shell语言编写的批处理文件，可作为Shell命令运行