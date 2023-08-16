#nxp_s32g3

This repository contains arm-trusted-firmware and U-Boot image, source code and patches for S32G-VNP-RDB3 board.

## fip.s32-sdcard

The file `fip.s32-sdcard` is a disk image.
dd command can be used for low-level copying the data of this file to the SD card device as below:

```
$ sudo dd if=./fip.s32-sdcard of=/dev/sdb conv=notrunc,fsync seek=512 skip=512 oflag=seek_bytes iflag=skip_bytes && sync
```
NOTE: This command assumes the SD card drive name is /dev/sdb.

## arm-trusted-firmware-nxp_s32g3.tar.gz

The file arm-trusted-firmware-nxp_s32g3.tar.gz is the arm-trusted-firmware source code.
It's based on NXP Auto Linux BSP(auto_yocto_bsp) release/bsp36.0 with patches meta-tfa-patches-nxp_s32g3.tar.gz added.

## u-boot_nxp_s32g3.tar.gz

The file u-boot_nxp_s32g3.tar.gz is the U-Boot source code.
It's based on NXP Auto Linux BSP(auto_yocto_bsp) release/bsp36.0 with XEN enabled.

## meta-tfa-patches-nxp_s32g3.tar.gz

The file meta-tfa-patches-nxp_s32g3.tar.gz is patch file used to patch arm-trusted-firmware. You can use it to rebuild arm-trusted-firmware.
To rebuild U-Boot you need to have `repo` installed on Linux and its prerequisites.

**Running the below command would be enough to completely build arm-trusted-firmware and U-Boot to be deployed.**

```
$ mkdir fsl-auto-yocto-bsp36

$ cd fsl-auto-yocto-bsp36

$ repo init -u https://github.com/nxp-auto-linux/auto_yocto_bsp -b release/bsp36.0

$ repo sync

$ tar -xzvf ./meta-tfa-patches-nxp_s32g3.tar.gz -C <Patch Folder>

$ mkdir sources/meta-alb/recipes-bsp/arm-trusted-firmware/arm-trusted-firmware

$ cp <Patch Folder>/0005-nxp-s32g-atf-add-xrdc-scmi.patch sources/meta-alb/recipes-bsp/arm-trusted-firmware/arm-trusted-firmware

$ echo "SRC_URI += \"file://0005-nxp-s32g-atf-add-xrdc-scmi.patch \"" >> sources/meta-alb/recipes-bsp/arm-trusted-firmware/arm-trusted-firmware_2.5.bb

$ ./sources/meta-alb/scripts/host-prepare.sh

$ bitbake arm-trusted-firmware

```
NOTE: <Patch Folder> is the location where to save the patch file

The built image `fip.s32-sdcard` can be found in `build_s32g399ardb3/tmp/deploy/images/s32g399ardb3`.
