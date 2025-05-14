<!--
// Filename: README.md
// Author: Olivier Sirol <czo@free.fr>
// License: GPL-2.0 (http://www.gnu.org/copyleft)
// File Created: nov. 2018
// Last Modified: Wednesday 14 May 2025, 22:13
// Edit Time: 2:28:53
-->

# My BusyBox for OpenWRT

Recompile BusyBox with
 arping,
 chpasswd,
 cksum,
 diff,
 find (seems to be in owrt 24.10),
 hostname,
 shred,
 ssty,
 tac,
 telnet,
 tty,
 xxd,
 editing savehistory and busybox applet, that are not provided with openwrt's busybox.

## Binaries for TP-Link Archer C7 v2

You can download my new busybox for TP-Link Archer C7 v2 on my [Releases page](https://github.com/czodroid/openwrt-busybox-czo/releases).

Copy it to your OpenWRT
 `scp busybox_1.36.1-r42_mips_24kc.ipk root@sw-marion:/tmp/`
and install it on your router
 `opkg install /tmp/busybox_1.36.1-r42_mips_24kc.ipk`
and `reboot` it!

### Size

Doing a `ls -al overlay/upper/bin/busybox rom/bin/busybox` you can know the size of BusyBox.

For busybox 1.36.1 on OpenWrt 24.10.1, its size is 20% bigger:

```
-rwxr-xr-x 1 root root 393253 2025-04-10 20:51 overlay/upper/bin/busybox
-rwxr-xr-x 1 root root 327717 2025-04-13 18:38 rom/bin/busybox
```

I don't know why the openwrt team doesn't add these commands... but I'd like to know.

## Compiling

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.


### Quickstart for TP-Link Archer C7 v2

For busybox 1.36.1 on OpenWrt 24.10.1

Download the SDK, untar it, mv it to a small name, and cd to it. Then run `feeds` to obtain all the latest package definitions and get busybox, then run `usign` to get a key-build, then copy .config.ow.czo (my defition of BusyBox), then make!

```
wget https://downloads.openwrt.org/releases/24.10.1/targets/ath79/generic/openwrt-sdk-24.10.1-ath79-generic_gcc-13.3.0_musl.Linux-x86_64.tar.zst
tar -xf openwrt-sdk-24.10.1-ath79-generic_gcc-13.3.0_musl.Linux-x86_64.tar.zst
mv openwrt-sdk-24.10.1-ath79-generic_gcc-13.3.0_musl.Linux-x86_64 owrt

cd owrt
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"

# stop here for devel

perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=42,' package/feeds/base/busybox/Makefile
cp ../.config.ow.czo .config
make package/busybox/compile
```

The package is in `bin/packages/mips_24kc/base/busybox_1.36.1-42_mips_24kc.ipk`.

Copy it to your OpenWRT
 `scp bin/packages/mips_24kc/base/busybox_1.36.1-42_mips_24kc.ipk root@sw-marion:/tmp/`
and install it on your router
 `opkg install /tmp/busybox_1.36.1-42_mips_24kc.ipk`
and `reboot` it!

## Development

### Start from Quickstart

and then copy it:

```
cd ..
rsync -av owrt/ ooo
cd owrt
```

and,

```
make menuconfig
```

In the confid, choose:

Base system -->\
\[\*\] Customize busybox options -->\
Settings  -->\
\[\*\] Include busybox applet\
Coreutils  --->\
\[\*\] cksum (4.1 kb) (NEW)

then at line ~3820, CONFIG_BUSYBOX_CONFIG_HAVE_DOT_CONFIG=y, and go for diff!

```
vimdiff .config ../.config.ow.czo
```

### Make

```
perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=30,' package/feeds/base/busybox/Makefile
cp -f ../.config.ow.czo .config
make package/busybox/compile
```

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


