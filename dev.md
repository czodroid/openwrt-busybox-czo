<!--
Filename: dev.md
Author: Olivier Sirol <czo@free.fr>
License: GPL-2.0 (http://www.gnu.org/copyleft)
File Created: nov. 2018
Last Modified: Sunday 23 October 2022, 12:27
Edit Time: 0:05:44
-->

# start


```
cd owrt
./scripts/feeds update -a
./scripts/feeds install busybox
./staging_dir/host/bin/usign -G -s ./key-build -p ./key-build.pub -c "Local build key"
```

```
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
perl -i -pe 's,^PKG_RELEASE:=.*$,PKG_RELEASE:=52,' package/feeds/base/busybox/Makefile
make package/busybox/compile
```

# verify

```
busybox
ls -al /rom/bin/busybox /overlay/upper/bin/busybox
```
