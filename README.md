<!--
Filename: README.md
Author: Olivier Sirol <czo@free.fr>
License: GPL-2.0 (http://www.gnu.org/copyleft)
File Created: nov. 2018
Last Modified: Wednesday 15 November 2023, 20:22
Edit Time: 2:16:32
-->

## New BusyBox for OpenWRT

Recompile BusyBox with
 cksum,
 shred,
 ssty,
 tty,
 diff,
 chpasswd,
 xxd,
 arping,
 hostname,
 telnet,
 editing savehistory
 and busybox applet, that are not provided with openwrt's busybox.

### Size

Doing a `ls -al /rom/bin/busybox /overlay/upper/bin/busybox` you can know the size of BusyBox.

For busybox 1.35.0 on OpenWrt 23.05.0, its size is 1.2% bigger,

```
-rwxr-xr-x 1 root root 327717 2023-04-27 23:05 overlay/upper/bin/busybox
-rwxr-xr-x 1 root root 323621 2023-04-27 22:28 rom/bin/busybox
```

I don't know why the openwrt team doesn't add these commands...


## Compiling

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.


### Quickstart for TP-Link Archer C7 v2

For busybox 1.35.0 on OpenWrt 23.05.0

Download the SDK, untar it, mv it to a small name, and cd to it. Then run `feeds` to obtain all the latest package definitions and get busybox, then run `usign` to get a key-build, then copy .config.ow.czo (my defition of BusyBox), then make!

```
wget https://downloads.openwrt.org/releases/23.05.0/targets/ath79/generic/openwrt-sdk-23.05.0-ath79-generic_gcc-12.3.0_musl.Linux-x86_64.tar.xz
tar -xf openwrt-sdk-23.05.0-ath79-generic_gcc-12.3.0_musl.Linux-x86_64.tar.xz
mv openwrt-sdk-23.05.0-ath79-generic_gcc-12.3.0_musl.Linux-x86_64 owrt
cd owrt
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"
perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=42,' package/feeds/base/busybox/Makefile

cp ../.config.ow.czo .config
make package/busybox/compile
```

The package is in `bin/packages/mips_24kc/base/busybox_1.35.0-42_mips_24kc.ipk`.

Copy it to your OpenWRT
 `scp bin/packages/mips_24kc/base/busybox_1.35.0-42_mips_24kc.ipk root@sw-marion:/tmp/`
and install it
 `opkg install /tmp/busybox_1.35.0-42_mips_24kc.ipk`
  !!!

## OpenWrt links

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0


