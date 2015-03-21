# SimpleScalar.md

http://www3.nd.edu/~mniemier/teaching/2010_B_Fall/labs/simplescalar.html

因为编译内核的需要， Ubuntu 自带的 gcc4.3 版本太高，需要使用 gcc3.x ，因此需要安装低版本的 gcc ，我选择的是 gcc3.4.4.

（ 1 ）下载 deb 安装包，我下载的包为：

gcc-3.4-base_3.4.6-6ubuntu3_i386.deb 、

gcc-3.4_3.4.6-6ubuntu3_i386.deb 、

cpp-3.4_3.4.6-6ubuntu3_i386.deb 、

g++-3.4_3.4.6-6ubuntu3_i386.deb 、

libstdc++6-dev_3.4.6-6ubuntu3_i386.deb

下载地址为： http://archive.ubuntu.com/ubuntu/pool/universe/g/gcc-3.4/
http://old-releases.ubuntu.com/ubuntu/pool/universe/g/gcc-3.4/

（ 2 ）安装这些包

sudo dpkg –force-depends –i xxx.deb

（ 3 ）系统配置

安装完成之后，在系统里会多出： gcc-3.4

目前系统里有两个版本的 gcc ，缺省时 gcc4.3 ；需要改变系统的缺省配置：

增加 gcc3.4 和 gcc4.3 可选项

$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.3 40

$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-3.4 30

切换版本到 gcc-3.4

$ sudo update-alternatives --config gcc

现有 2 个可选项，它们都提供了“ gcc ”

<    选择         可选项 -----------------------------------------------

*+1    /usr/bin/gcc-3.4

2    /usr/bin/gcc-4.2

要维持缺省值 [*] ，按回车键，或者键入选择的编号： 1

使用“ /usr/bin/gcc-3.4 ”来提供“ gcc ”。

至此编译成功。
