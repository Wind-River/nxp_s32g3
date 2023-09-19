#nxp_s32g3

This repository contains arm-trusted-firmware and U-Boot image, source code and patches for S32G-VNP-RDB3 board.

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

**Legal Notices**
All product names, logos, and brands are property of their respective owners. All company, product and service names used in this software are for identification purposes only. Wind River and VxWorks are registered trademarks of Wind River Systems, Inc. UNIX is a registered trademark of The Open Group.
Disclaimer of Warranty / No Support: Wind River does not provide support and maintenance services for this software, under Wind River’s standard Software Support and Maintenance Agreement or otherwise. Unless required by applicable law, Wind River provides the software (and each contributor provides its contribution) on an “AS IS” BASIS, WITHOUT WARRANTIES OF ANY KIND, either express or implied, including, without limitation, any warranties of TITLE, NONINFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A PARTICULAR PURPOSE. You are solely responsible for determining the appropriateness of using or redistributing the software and assume any risks associated with your exercise of permissions under the license.
