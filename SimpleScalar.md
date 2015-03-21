# SimpleScalar.md

## Ubuntu12.04 i386 System Env

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

然后随意写个helloworld程序，尝试编译一下 gcc -g helloworld.c

如果不能通过，哈哈～你跟我一样倒霉～

/usr/bin/ld: cannot find crt1.o
ln -sf /usr/lib/i386-linux-gnu/crt1.o /usr/lib/crt1.o
可能是双版本gcc的原因，需要将/usr/lib/i386-linux-gnu下的crt1.o 设置链接到/usr/lib/下

/usr/bin/ld: cannot find -lgcc_s
/usr/lib/gcc/i486-linux-gnu/3.4.6/文件夹下查找libgcc_s.so文件。libgcc_s.so是一个链接文件，链接到对应目录的libgcc_s.so.1文件。打开libgcc_s.so弹出提示链接已损坏

先定向libgcc_s.so.1,然后重新设置链接

locate libgcc_s.so.1      
ln -sf /lib/i386-linux-gnu/libgcc_s.so.1/usr/lib/gcc/i486-linux-gnu/3.4.6/libgcc_s.so
还有个链接也有问题，同理处理

里面提到的几个问题都解决了，在编译Helloworld的时候还遇到了头文件的问题，打出了一堆错误信息，不贴了，解决方法：
export LIBRARY_PATH=/usr/lib/i386-linux-gnu
export C_INCLUDE_PATH=/usr/include/i386-linux-gnu
export CPLUS_INCLUDE_PATH=/usr/include/i386-linux-gnu

## SimpleScalar Admin https://wiki.cse.buffalo.edu/services/content/simplescalararm-admin
Introduction

simplesim-arm is not tested or guaranteed to work on 64-bit Linux systems (like ours). But it seems to work. The trick is to make the source directory and build directory the same directory (denoted by --prefix below)..
Download

From the University of Michigan's SimpleScalar website, download:
SimpleScalar/ARM
SimpleScalar/ARM cross-compiler kit
Prepare

On your build/distribution system, verify or install gcc 3.4:

% yum list installed | grep compat-gcc-34
compat-gcc-34.x86_64                     3.4.6-4.1                     installed
% which gcc34
/usr/bin/gcc34

We need to build everything with gcc34. Set environment variables accordingly:

% setenv CC gcc34
% setenv HOST_CC gcc34

Build

Build SimpleScalar/ARM

Unpack the distribution:

% gunzip simplesim-arm-0.2.tar.gz
% tar -xvf simplesim-arm-0.2.tar
% cd simplesim-arm

Follow Hao Wang's advice to comment out error-producing cygwin 'if' clauses.
Make:

simplesim-arm% make config-arm
simplesim-arm% make "CC=gcc34"

Build SimpleScalar/ARM cross-compiler kit

Build binutils

Configure, Make, Make Install:

binutils-2.10% ./configure --target=arm-linux --host=arm-linux --prefix=/util/simplesim/crossdir
binutils-2.10% make
binutils-2.10% sudo make install

Add the crossdir binaries directory to your executable path to get ready for the next build:

% setenv PATH ${PATH}:/util/simplesim/crossdir/bin
% rehash

Build GNU GCC

Configure, Make, Make Install:

gcc-2.95.2% ./configure --target=arm-linux --host=arm-linux --prefix=/util/simplesim/crossdir
gcc-2.95.2% make LANGUAGES=c
gcc-2.95.2% sudo make LANGUAGES=c install
gcc-2.95.2% cd ..
crossdir% sudo vi lib/gcc-lib/arm-linux/2.95.2/specs

[replace all occurrences of "elf32arm" with "armelf_linux"]

References

http://www.simplescalar.com/v4test.html
http://web.eecs.umich.edu/~taustin/code/arm/ANNOUNCE.ARM
http://web.eecs.umich.edu/~taustin/code/arm-cross/ANNOUNCE.cross
https://sites.google.com/site/pkuwangh/solutions/simplescalar
