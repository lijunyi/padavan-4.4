# Padavan-4.4 Project Overview

本项目是一个基于 Padavan 的开源固件编译系统，采用了 Linux 4.4.198 内核，旨在为多种 MIPS (mt7621) 架构的路由器（如小米 CR660x、斐讯 K2P 等）提供高性能、功能丰富的定制固件。

## 核心技术栈
- **内核**: Linux Kernel 4.4.198 (来自 D-LINK GPL 代码)
- **架构**: MIPS (mipsel-linux-uclibc)
- **编译系统**: GNU Make, Shell Scripts
- **工具链**: crosstool-ng (mipsel-linux-uclibc)
- **组件**: BusyBox, iptables, dnsmasq, dropbear, WireGuard, SFE (Shortcut Forwarding Engine) 等。

## 目录结构
- `/toolchain-mipsel/`: 交叉编译工具链的构建与管理目录。
- `/trunk/`: 固件编译的主目录。
    - `configs/templates/`: 各类路由器的默认编译配置文件（`.config`）。
    - `configs/boards/`: 具体板级的硬件配置，包括 `board.h` 和内核配置。
    - `linux-4.4.x/`: Linux 内核源码。
    - `user/`: 用户态应用程序源码（如 httpd, busybox, samba 等）。
    - `libs/`: 编译固件所需的第三方库源码。
    - `vendors/`: 厂商/板级相关的初始化和配置文件。
    - `images/`: 编译生成的固件镜像文件。

## 编译指南

### 1. 准备工作
首先安装必要的依赖（以 Ubuntu/Debian 为例）：
```bash
sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd \
    fakeroot kmod cpio git python3-docutils gettext automake autopoint \
    texinfo build-essential help2man pkg-config zlib1g-dev libgmp3-dev \
    libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev wget libc-dev-bin
```

### 2. 准备工具链
推荐使用预编译的工具链以节省时间：
```bash
cd toolchain-mipsel
./dl_toolchain.sh
```
或者手动从源码编译（非常耗时）：
```bash
./build_toolchain
```

### 3. 编译固件
进入 `trunk` 目录，选择一个设备模板进行编译：
```bash
cd trunk
# 以斐讯 K2P 为例
fakeroot ./build_firmware_modify K2P
```
编译完成后，生成的固件将存放在 `trunk/images/` 目录下。

### 4. 清理编译环境
在更换设备编译前，建议清理旧的编译产物：
```bash
cd trunk
./clear_tree
```

## 开发约定与最佳实践
- **配置文件**: 优先修改 `trunk/configs/templates/*.config` 来启用或禁用功能。
- **板级支持**: 若需支持新设备，需在 `trunk/configs/boards/` 下创建对应的子目录，并配置 `board.h` 和内核 config。
- **添加应用**: 新的应用程序应放置在 `trunk/user/` 下，并在 `trunk/user/Makefile` 中添加相应的编译项。
- **内核修改**: 针对内核的修改应在 `trunk/linux-4.4.x/` 中进行，并注意不同板级的内核 config 差异。
- **调试**: 固件运行时的日志通常可以通过 `dmesg` 或 `logread` 查看。

## 注意事项
- 编译过程必须使用 `fakeroot`（在 WSL 下推荐 `fakeroot-tcp` 或 `sudo`）。
- 请确保 `toolchain-mipsel` 目录在 `trunk` 目录的同级位置。
