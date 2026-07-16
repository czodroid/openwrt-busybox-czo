<!--
// Filename: README.md
// Author: Olivier Sirol <czo@free.fr>
// License: GPL-2.0 (http://www.gnu.org/copyleft)
// File Created: nov. 2018
// Last Modified: Thursday 16 July 2026, 20:48
// Edit Time: 6:38:26
-->

# My BusyBox for OpenWRT

Recompile BusyBox with the following applets that are not included in OpenWrt's
default BusyBox:

```
arping
chpasswd
cksum
diff
find (seems to be in owrt 24.10)
hostname
shred
ssty
tac
telnet
tty
xxd
pgrep
pkill
sh with editing savehistory, reverse search, and better terminal
and busybox applet
```

## Binaries for TP-Link Archer C7 v2

You can download my new busybox for TP-Link Archer C7 v2 on my
[Releases page](https://github.com/czodroid/openwrt-busybox-czo/releases).

Copy it to your OpenWRT
 `scp busybox-1.37.0-r42.apk root@sw-marion:/tmp/`
and install it on your router
 `apk add --allow-untrusted /tmp/busybox-1.37.0-r42.apk`
and `reboot` it!

> Note: it is not an opkg .ipk, it is an .apk for apk tools that the new release
of OpenWRT uses, borrowed from Alpine Linux.

### Size

Doing a `ls -al overlay/upper/bin/busybox rom/bin/busybox` you can know the size of BusyBox.

For busybox 1.37.0 on OpenWrt 25.12.5, its size is 8% bigger:

```
-rwxr-xr-x 1 root root 393253 2026-05-05 00:48 overlay/upper/bin/busybox
-rwxr-xr-x 1 root root 327717 2026-05-05 00:30 rom/bin/busybox
```

I don't know why the openwrt team doesn't add these commands... but I'd like to know.

## Compiling

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.


### Quickstart for TP-Link Archer C7 v2

Download the SDK, untar it, rename it to a shorter name, and cd to it. Then run `feeds`
to obtain all the latest package definitions and get busybox, then run `usign` to
get a key-build, then copy .config.ow.czo (my defition of BusyBox), then make!

```
wget https://downloads.openwrt.org/releases/25.12.5/targets/ath79/generic/openwrt-sdk-25.12.5-ath79-generic_gcc-14.3.0_musl.Linux-x86_64.tar.zst
tar -xf openwrt-sdk-25.12.5-ath79-generic_gcc-14.3.0_musl.Linux-x86_64.tar.zst
mv openwrt-sdk-25.12.5-ath79-generic_gcc-14.3.0_musl.Linux-x86_64 owrt
cd owrt
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"

# stop here for devel

perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=42,' package/feeds/base/busybox/Makefile
cp ../.config.ow.czo .config
make package/busybox/compile
```

The new package is in `bin/packages/mips_24kc/base/busybox-1.37.0-r42.apk`.

## Development

### Start from Quickstart and then

backup:

```
cd ..
rsync -av owrt/ ooo
cd owrt
```

and:

```
make menuconfig
```

In the config, choose:

```
Base system -->
    [*] Customize busybox options -->
    Settings  -->
        [*] Include busybox applet
    Coreutils  -->
        [*] cksum (4.1 kb) (NEW)
```

then at line ~4000, CONFIG_BUSYBOX_CONFIG_HAVE_DOT_CONFIG=y, and go for diff!

```
vim -d .config ../.config.ow.czo
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

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advice.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0


