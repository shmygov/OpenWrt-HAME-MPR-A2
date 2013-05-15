OpenWrt-HAME-MPR-A2
==============

Patch to compile OpenWrt Linux (latest trunk versions which use "Device Trees") for "HAME MPR-A2" Ralink RT5350-based router

## Introduction

"HAME MPR-A2" manufacturer's web page:

     http://www.hametech.com

"HAME MPR-A2" is similar to "HAME MPR-A1", but it has 8MB Flash (s25fl064k), 32MB RAM, and bigger 5200 mAh battery.

## Build Instructions

In order to build OpenWrt for "HAME MPR-A2", you need to:
* download the latest OpenWrt trunk sources from svn
* download the patch
* apply the patch
* choose your target/subtarget/profile for the build
* compile the firmware

This is achieved using the following code snippet:

     mkdir openwrt
     cd openwrt
     svn co svn://svn.openwrt.org/openwrt/trunk
     git clone https://github.com/shmygov/OpenWrt-HAME-MPR-A2.git
     cd trunk
     patch -p0 <../OpenWrt-HAME-MPR-A2/openwrt_add_support_for_hame_mpr_a2.patch

Empty the openwrt/trunk/tmp folder and then run:

     make menuconfig

In the configuration menu, you need to select the following options:

* Target System: Ralink RT288x/RT3xxx
* Subtarget: RT305x based boards
* Target Profile: HAME MPR-A2

To flash the OpenWrt image to HAME MPR-A2 running factory firmware using the factory Web UI you will need "initramfs" type image.
To compile such image, select the following option:

* Target Images: ramdisk

After the build completes the firmware file will be located in the openwrt/trunk/bin/ramips/ folder, file name:
     openwrt-ramips-rt305x-mpr-a2-initramfs-uImage.bin

After intalling the "initramfs" image, you can upgrade to the recommended "squashfs" type image.
To compile such image, instead of "initramfs" select the default "squashfs" option:

* Target Images: squashfs

After the build completes the firmware file will be located in the openwrt/trunk/bin/ramips/ folder, file name:
     openwrt-ramips-rt305x-mpr-a2-squashfs-sysupgrade.bin

To use OpenWrt with LuCI Web UI, you can additionally select following options:

* LuCI --> Collections --> luci
* LuCI --> Protocols --> luci-proto-3g

After all the needed options are selected, exit the menu, save the configuration, and proceed to build:

     make

## Patch Contents

### openwrt_add_support_for_hame_mpr_a2.patch

This patch is based on previous work by Squonk42 (https://github.com/Squonk42/OpenWrt-RT5350) and others from OpenWrt forum (https://forum.openwrt.org/viewtopic.php?id=37002&p=19), adapted for the new "Device Trees" structure based on dts files used in the latest OpenWrt trunk.

