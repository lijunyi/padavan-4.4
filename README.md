# padavan-4.4 #

This project is based on original rt-n56u with latest mtk 4.4.198 kernel, which is fetch from D-LINK GPL code.

- Features
  - Based on 4.4.198 Linux kernel
  - Support MT7621 based devices
  - Support MT7615D/MT7615N/MT7915D wireless chips
  - Support raeth and mt7621 hwnat with legency driver
  - Support qca shortcut-fe
  - Support IPv6 NAT based on netfilter
  - Support WireGuard integrated in kernel
  - Support fullcone NAT (by Chion82)
  - Support LED&GPIO control via sysfs

- Component Upgrades (Updated 2026-03-10)
  - **BusyBox**: 1.24.x -> [1.36.1](https://busybox.net/downloads/busybox-1.36.1.tar.bz2)
  - **dnsmasq**: 2.8x -> [2.90](https://thekelleys.org.uk/dnsmasq/dnsmasq-2.90.tar.xz)
  - **Dropbear**: 2020.81 -> [2024.86](https://matt.ucc.asn.au/dropbear/releases/dropbear-2024.86.tar.bz2) (Enabled Curve25519 & GCM)
  - **OpenSSL**: 1.1.1k -> [1.1.1w](https://www.openssl.org/source/openssl-1.1.1w.tar.gz) (With MIPS speed optimization)
  - **cURL**: 7.75.0 -> [8.7.1](https://curl.se/download/curl-8.7.1.tar.xz)
  - **IPSet**: 7.11 -> [7.23](https://ipset.netfilter.org/ipset-7.23.tar.bz2) (Fixed kernel compatibility)
  - **iptables**: 1.8.7 -> [1.8.10](https://www.netfilter.org/projects/iptables/files/iptables-1.8.10.tar.xz)
  - **iproute2**: 5.12.0 -> [5.15.0](https://mirrors.edge.kernel.org/pub/linux/utils/net/iproute2/iproute2-5.15.0.tar.xz)
  - **libz**: 1.2.11 -> [1.3.1](https://github.com/madler/zlib/releases/download/v1.3.1/zlib-1.3.1.tar.gz)
  - **libmnl**: 1.0.4 -> [1.0.5](https://www.netfilter.org/projects/libmnl/files/libmnl-1.0.5.tar.bz2)
  - **libpcap**: 1.9.1 -> [1.10.4](http://www.tcpdump.org/release/libpcap-1.10.4.tar.gz)
  - **htop**: 3.0.2 -> [3.3.0](https://github.com/htop-dev/htop/releases/download/3.3.0/htop-3.3.0.tar.xz)
  - **nano**: 3.0 -> [8.0](https://www.nano-editor.org/dist/v8/nano-8.0.tar.xz)
  - **mtr**: 0.92 -> [0.95](https://github.com/traviscross/mtr/archive/v0.95.tar.gz)
  - **socat**: 1.7.3.3 -> [1.8.0.1](http://www.dest-unreach.org/socat/download/socat-1.8.0.1.tar.gz)

- WIP
  - 802.11kvr and mtkiappd roam functions
  - IPTV related functions

- Supported devices
  - CR660x
  - JCG-Q20
  - JCG-AC860M
  - JCG-836PRO
  - JCG-Y2
  - DIR-878
  - DIR-882
  - K2P
  - K2P-USB
  - NETGEAR-BZV
  - MR2600
  - MI-R3P
  - XY-C1

- Compilation step
  - Install dependencies
    ```sh
    # Debian/Ubuntu
    sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd \
        fakeroot kmod cpio git python3-docutils gettext automake autopoint \
        texinfo build-essential help2man pkg-config zlib1g-dev libgmp3-dev \
        libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev wget libc-dev-bin

    # Archlinux/Manjaro
    sudo pacman -Syu --needed git base-devel cmake gperf ncurses libmpc \
            gmp python-docutils vim rpcsvc-proto fakeroot cpio help2man

    # Alpine
    sudo apk add make gcc g++ cpio curl wget nano xxd kmod \
        pkgconfig rpcgen fakeroot ncurses bash patch \
        bsd-compat-headers python2 python3 zlib-dev \
        automake gettext gettext-dev autoconf bison \
        flex coreutils cmake git libtool gawk sudo
    ```
  - Clone source code
    ```sh
    git clone https://github.com/hanwckf/padavan-4.4.git
    ```
  - Prepare toolchain
    ```sh
    cd padavan-4.4/toolchain-mipsel

    # (Recommend) Download prebuilt toolchain for x86_64 or aarch64 host
    ./dl_toolchain.sh

    # or build toolchain with crosstool-ng
    # ./build_toolchain
    ```
  - Modify template file and start compiling
    ```sh
    cd padavan-4.4/trunk

    # (Optional) Modify template file
    # nano configs/templates/K2P.config

    # Start compiling
    fakeroot ./build_firmware_modify K2P

    # To build firmware for other devices, clean the tree after previous build
    ./clear_tree
    ```

- Manuals
  - Controlling GPIO and LEDs via sysfs
  - How to use NAND RWFS partition
  - How to use IPv6 NAT and fullcone NAT
  - How to add new device support with device tree
