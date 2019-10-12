# crosstool-ng
crossool-ng binary file &amp; config file


## 功能说明
crosstool-ng工具制作，基于ubuntu 16.04 64 bit系统  

## 文件/目录说明
* download：已经下载好的软件包（文件较多，体积较大，酌情考虑是否下载）

* .config：本书已经配置过的配置文件，供参考

* crosstool-ng-1.23.0.tar.xz：官方软件安装包

* crosstool-ng-bin.tar.bz2：编译好的ct-ng目录压缩包，解压后文件目录说明：
```
.
├── bin
│   └── ct-ng # 制作工具
├── lib
│   └── crosstool-ng-1.23.0
│       ├── config # 编译时需要的配置文件
│       ├── contrib
│       ├── kconfig
│       ├── patches
│       ├── paths.mk
│       ├── paths.sh
│       ├── samples # 编译器配置示例，本书使用的是arm-unknown-linux-gnueabi
│       ├── scripts
│       └── steps.mk
└── share
    ├── doc
    │   └── crosstool-ng
    └── man
        └── man1
```

## 使用说明
### 1、安装必备工具
参考本书第二章节，安装必备工具。如果依然有问题，则执行：  
```
$ sudo apt-get install -y bison flex gperf texinfo gawk libtool automake libncurses5-dev help2man
```  

### 2、解压
解压crosstool-ng-bin.tar.bz2到/opt/ellp目录。（一定是这个目录，因为本书编译crosstool-ng源码时就使用/opt/ellp目录，如没有则创建，后续命令如提示权限不够，则使用sudo执行）
```
$ sudo mkdir /opt/ellp  
$ sudo cp crosstool-ng-bin.tar.bz2 /opt/ellp  
$ cd /opt/ellp  
$ sudo tar jxf crosstool-ng-bin.tar.bz2  
$ cd crosstool-ng
```
### 3、配置
拷贝官方的配置模板：
```
$ cp lib/crosstool-ng-1.23.0/samples/arm-unknown-linux-gnueabi/crosstool.config .config
```
也可以使用本目录自带的文件`.config`。
配置：  
```
$ ./bin/ct-ng menuconfig
```

注：如果提示：
```
-bash: ./ct-ng: Permission denied
```
则执行：
```
chmod +x cg-ng
```

### 软件包目录设置（可选步骤）
由于crosstool-ng需要联网下载软件，本工程已有，如果需要，则将download目录所有文件拷贝到~/src目录下（不存在则创建，因为crosstool-ng默认使用该目录）。

### 4、编译
编译命令：  
```
$ ./bin/ct-ng build
```
（注：这一步骤耗时较久，半小时到1小时不等。）

### 5、生成文件

最终生成文件位于~/x-tools（默认安装目录）。
进入该目录验证版本号：  
```
$ ./arm-unknown-linux-gnueabihf/bin/arm-linux-gcc -v  
Using built-in specs.
COLLECT_GCC=./bin/arm-linux-gcc
COLLECT_LTO_WRAPPER=/home/latelee/x-tools/arm-unknown-linux-gnueabihf/libexec/gcc/arm-unknown-linux-gnueabihf/6.3.0/lto-wrapper
Target: arm-unknown-linux-gnueabihf
Configured with: /opt/ellp/crosstool-ng/.build/src/gcc-6.3.0/configure --build=x86_64-build_pc-linux-gnu --host=x86_64-build_pc-linux-gnu --target=arm-unknown-linux-gnueabihf --prefix=/home/latelee/x-tools/arm-unknown-linux-gnueabihf --with-sysroot=/home/latelee/x-tools/arm-unknown-linux-gnueabihf/arm-unknown-linux-gnueabihf/sysroot --enable-languages=c,c++ --with-fpu=neon --with-float=hard --with-pkgversion='crosstool-NG crosstool-ng-1.23.0' --disable-sjlj-exceptions --enable-__cxa_atexit --disable-libmudflap --disable-libgomp --disable-libssp --disable-libquadmath --disable-libquadmath-support --disable-libsanitizer --disable-libmpx --with-gmp=/opt/ellp/crosstool-ng/.build/arm-unknown-linux-gnueabihf/buildtools --with-mpfr=/opt/ellp/crosstool-ng/.build/arm-unknown-linux-gnueabihf/buildtools --with-mpc=/opt/ellp/crosstool-ng/.build/arm-unknown-linux-gnueabihf/buildtools --with-isl=/opt/ellp/crosstool-ng/.build/arm-unknown-linux-gnueabihf/buildtools --enable-lto --with-host-libstdcxx='-static-libgcc -Wl,-Bstatic,-lstdc++,-Bdynamic -lm' --enable-threads=posix --enable-target-optspace --enable-plugin --enable-gold --disable-nls --disable-multilib --with-local-prefix=/home/latelee/x-tools/arm-unknown-linux-gnueabihf/arm-unknown-linux-gnueabihf/sysroot --enable-long-long
Thread model: posix
gcc version 6.3.0 (crosstool-NG crosstool-ng-1.23.0) 
```

### 6、配置环境变量
1、将`export PATH=$HOME/x-tools/arm-unknown-linux-gnueabihf/bin:$PATH`写到`~/.bashrc`文件末尾。

2、执行`source ~/.bashrc`或重启机器让交叉编译器生效。  

