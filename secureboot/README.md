#secureboot

This repository contains arm-trusted-firmware and U-Boot patches which used for S32G-VNP-RDB3 board secure boot.

## 0001-nxp-s32g3-uboot-enable-secureboot.patch
The file 0001-nxp-s32g3-uboot-enable-secureboot.patch is patch file used to patch U-Boot. You can use it to rebuild U-Boot to support secure boot.

**Running the below command would be enough to completely build U-boot with secure boot enabled.**

```
$ git clone https://github.com/nxp-auto-linux/u-boot
$ cd  u-boot
$ git checkout  bsp36.0-2020.04
$ git revert fc931960b574f9019ee71a04145419474ccee1b8
$ cp <Patch Folder>/0001-nxp-s32g3-uboot-enable-secureboot.patch .
$ patch -p 1 <0001-nxp-s32g3-uboot-enable-secureboot.patch

$ vim configs/s32g399ardb3_defconfig
$ ### Add the following lines:
    CONFIG_XEN_SUPPORT=y
    CONFIG_OF_CONTROL=y
    CONFIG_FIT_SIGNATURE=y
    CONFIG_NXP_HSE_SUPPORT=y
    CONFIG_NXP_HSE_FW_FILE="<HSE bin Folder>/rev1.0_s32g3xx_hse_fw_0.20.0_2.16.1_pb221011.bin.pink"

$ make CROSS_COMPILE=<Cross compliler Folder>/bin/aarch64-none-linux-gnu- s32g399ardb3_defconfig
$ export CROSS_COMPILE=<Cross compliler Folder>/bin/aarch64-none-linux-gnu-
$ make
```

## 0001-nxp-s32g3-atf-support-secureboot.patch
The file 0001-nxp-s32g3-uboot-enable-secureboot.patch is patch file used to patch arm-trusted-firmware. You can use it to rebuild arm-trusted-firmware
to support secure boot.

**Running the below command would be enough to completely build arm-trusted-firmware with secure boot enabled.**

```
$ git clone https://github.com/nxp-auto-linux/arm-trusted-firmware
$ cd arm-trusted-firmware
$ git checkout bsp36.0-2.5
$ cp <Patch Folder>/meta-tfa-patches-nxp_s32g3.tar.gz .
$ tar zxvf meta-tfa-patches-nxp_s32g3.tar.gz
$ cp <Patch Folder>/0001-nxp-s32g3-atf-support-secureboot.patch .
$ patch -p 1 <0005-nxp-s32g-atf-add-xrdc-scmi.patch
$ patch -p 1 <0001-nxp-s32g3-atf-support-secureboot.patch

$ make ARCH=aarch64 PLAT=s32g3xxaevb BL33=~/S32GG3/u-boot-36.0/u-boot-nodtb.bin HSE_SUPPORT=1 S32_HAS_HV=1
```

NOTE: <Patch Folder> is the location where to save the patch file
      <HSE bin Folder> is the location of HSE bin file which is published by NXP.
      <Cross compliler Folder>  is the location of ARM Cross compliler which can be download from (https://developer.arm.com/-/media/Files/downloads/gnu-a/10.2-2020.11/binrel/gcc-arm-10.2-2020.11-x86_64-aarch64-none-linux-gnu.tar.xz?revision=972019b5-912f-4ae6-864a-f61f570e2e7e&la=en&hash=B8618949E6095C87E4C9FFA1648CAA67D4997D88))
	  
For the sign steps of ATF image and how to get/write the final signed ATF image, you can reference the secure boot section of vxWorks NXP S32G3 BSP Supplements manual.
