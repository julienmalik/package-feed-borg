Build packages
==============

```
$ mkdir $HOME/turris
$ cd $HOME/turris
$ git clone https://github.com/julienmalik/package-feed-borg
$ git clone https://gitlab.nic.cz/turris/os/build turris-build
$ cd turris-build
$ echo "src-link borg $HOME/turris/package-feed-borg" >> feeds.conf
$ ./compile_pkgs prepare_tools -t omnia
$ cd build
$ scripts/feeds update -a
$ scripts/feeds install -a
$ make menuconfig
$ make package/borgbackup/compile
$ ls build/bin/packages/arm_cortex-a9_vfpv3-d16/borg -al
```


Install packages manually
=========================

```
$ scp -r build/bin/packages/arm_cortex-a9_vfpv3-d16/borg your_turris:/tmp
$ ssh your_turris
$ cd /tmp/borg
$ rm *-src_* xxhash_*
$ opkg --force-depends install *.ipk
```


Useful links
============

https://openwrt.org/docs/guide-developer/packages
https://notes.iopush.net/blog/2017/how-to-setup-an-openwrt-repo/
https://wiki.turris.cz/doc/en/public/custom_packages
https://forum.openwrt.org/t/how-to-include-additional-packages-using-cli-without-going-to-make-menuconfig/88558
