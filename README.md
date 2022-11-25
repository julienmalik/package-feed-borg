Introduction
============

I wanted to have the excellent borgbackup (https://borgbackup.readthedocs.io) installed on my Turris Omnia router (equipped with 2GB of RAM and SATA HDDs which should be sufficient for running a borg server).

I tried several things before:

- the armv7 standalone binaries provided at https://borg.bauerj.eu/ don't work (libc incompatibity)
- installing borg in a docker alpine container, did not work out either (RPC errors between client and server). I finally gave up, also because spawning a new container each time to run borg costs a lot on this router.

So I ended up crafting native packages.

This project came from my own need to make packages for Turris Omnia (currently 6.0, based off of OpenWRT 21.02), thus I only tested them on this platform.

However the packages should be compatible with stock OpenWRT (particularly 21.02) if you want to build them for your own router.

Build packages
==============

```
$ WORKDIR=$HOME/turris
$ mkdir $WORKDIR
$ cd $WORKDIR
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

- https://openwrt.org/docs/guide-developer/packages
- https://notes.iopush.net/blog/2017/how-to-setup-an-openwrt-repo/
- https://wiki.turris.cz/doc/en/public/custom_packages
- https://forum.openwrt.org/t/how-to-include-additional-packages-using-cli-without-going-to-make-menuconfig/88558
