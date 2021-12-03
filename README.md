
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
and editing savehistory, that are not provided with Openwrt.

For busybox 1.30.1 on OpenWrt 19.07.6, its size is 10% bigger, and I don't know why the openwrt team doesn't add
these commands...

```
-rwxr-xr-x 1 root root 299061 2021-01-19 14:10 ./rom/bin/busybox
-rwxr-xr-x 1 root root 327733 2021-01-19 14:10 ./overlay/upper/bin/busybox
```

## Compiling

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.


### Quickstart for TP-Link Archer C7 v2

For busybox 1.33.1 on OpenWrt 21.02.1

Download the SDK, untar it and cd to it. Then run `feeds` to obtain all the latest package definitions and get busybox, then run `usign` to get a key-build, then copy .config.ow.czo (my defition of BusyBox), then make!

```
wget https://downloads.openwrt.org/releases/21.02.1/targets/ath79/generic/openwrt-sdk-21.02.1-ath79-generic_gcc-8.4.0_musl.Linux-x86_64.tar.xz
tar -xf openwrt-sdk-21.02.1-ath79-generic_gcc-8.4.0_musl.Linux-x86_64.tar.xz
cd openwrt-sdk-21.02.1-ath79-generic_gcc-8.4.0_musl.Linux-x86_64
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"
cp ../.config.ow.czo .config
perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=99,' package/feeds/base/busybox/Makefile
make package/busybox/compile
```

The package is in `bin/packages/mips_24kc/base/busybox_1.33.1-99_mips_24kc.ipk`.
Copy it to your-router:/tmp and install it `opkg install busybox_1.33.1-99_mips_24kc.ipk` !!!

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


