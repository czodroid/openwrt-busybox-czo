<!--
Filename: dev.md
Author: Olivier Sirol <czo@free.fr>
License: GPL-2.0 (http://www.gnu.org/copyleft)
File Created: nov. 2018
Last Modified: Saturday 18 November 2023, 19:49
Edit Time: 0:21:36
-->

# start

get the right link:

```
wget https://downloads.openwrt.org/releases/23.05.2/targets/ath79/generic/openwrt-sdk-23.05.2-ath79-generic_gcc-12.3.0_musl.Linux-x86_64.tar.xz
tar -xf openwrt-sdk-23.05.2-ath79-generic_gcc-12.3.0_musl.Linux-x86_64.tar.xz
mv openwrt-sdk-23.05.2-ath79-generic_gcc-12.3.0_musl.Linux-x86_64 owrt
cd owrt
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"
cd ..
rsync -av owrt/ ooo
cd owrt
make menu-config
```

Base system -->
Customize busybox options -->
Coreutils -->
cksum \[*\]

```
vimdiff .config ../.config.ow.czo
```

# make

```
perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=30,' package/feeds/base/busybox/Makefile

cp -f ../.config.ow.czo .config
make package/busybox/compile
```

# verify

```
busybox
ls -al /rom/bin/busybox /overlay/upper/bin/busybox
```

