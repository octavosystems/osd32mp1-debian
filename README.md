# OSD32MP1 Debian SDK v3.0
This repository contains the list of repositories used for OSD32MP1 Debian SDK v3.0

## Introduction
OSD32MP1 Debian SDK can be used to develop Debian Linux images for OSD32MP1 based platforms. The SDK runs on host desktop preferably runnning Ubuntu. Docker is used to containerize the build environment. Souce code from ST (OpenSTLinux) is used, for TF-A, U-Boot, Linux kernel, GCNano and STM32CubeMP1. So, the SDK is compatible with software from ST. Binary images for both SD card and eMMC can be generated. Cube Programmer can be used to flash the eMMC via USB.

The following repositories make up OSD32MP1 Debian SDK::

| Package | URL |
| ------- | --- |
| TF-A Source code | https://github.com/STMicroelectronics/arm-trusted-firmware.git |
| U-Boot Source code | https://github.com/STMicroelectronics/u-boot.git |
| Linux kernel Source code | https://github.com/STMicroelectronics/linux.git |
| GCNano Binaries | https://github.com/STMicroelectronics/gcnano-binaries.git |
| STM32Cube MP1 Package | https://github.com/STMicroelectronics/STM32CubeMP1.git |
| Debian multistrap Source code | https://github.com/octavosystems/osd32mp1-multistrap.git |
| Docker image Source code |  https://github.com/octavosystems/osd32mp1-debian-docker.git |
| Build tools source code | https://github.com/octavosystems/osd32mp1-build-tools.git |

## Boards supported:
| Board | Webpage |
| ----- | ------- |
| OSD32MP1-RED | https://octavosystems.com/octavo_products/osd32mp1-red/ |

## Requirements
Tested on Ubuntu 20.04 LTS

The following packges are recommended:

| Package | Version |
| ------- | ------- |
| repo | v2.13 or higher|
| git | 2.17 or higher|
| Python | 3.x |
| Docker | 19.x |
| qemu-user-static | 1:4.2-3ubuntu6.19 |
| STM32MP1 Cube Programmer | 2.9 or higher|


## OSD32MP1 Target binary versions:

The following version of the image are supported in release:

| Package | Version | Source URL |
| ------- | ------- | ---------- |
| TF-A | v2.4-stm32mp-r1 | https://github.com/STMicroelectronics/arm-trusted-firmware.git | 
| U-Boot | 2020.10-stm32mp-r1 | https://github.com/STMicroelectronics/u-boot.git |
| Linux Kernel | v5.10-stm32mp-r1 | https://github.com/STMicroelectronics/linux.git |
| GCNano(binaries) | gcnano-6.4.3-binaries | https://github.com/STMicroelectronics/gcnano-binaries.git |
| STM32CubeMP1 | 1.4 | https://github.com/STMicroelectronics/STM32CubeMP1.git |

## How to use the SDK
### Setup Host computer
1. ```sudo apt-get update```
2. ```sudo apt-get upgrade```
3. ```sudo apt-get install git docker.io qemu-user-static```
4. ```sudo systemctl enable docker```
5. ```sudo systemctl start docker```
6. Install repo: https://stackoverflow.com/questions/4350344/how-to-install-androids-repo-on-my-ubuntu-system

Install Cube Programmer from here: https://www.st.com/en/development-tools/stm32cubeprog.html
Please see https://wiki.st.com/stm32mpu/wiki/STM32CubeProgrammer for Cube Programmer installation instructions and setup.

### Get SDK source code
1. ```mkdir ~/osd32mp1-workspace```
2. ```cd ~/osd32mp1-workspace```
3. ```repo init -u https://github.com/octavosystems/osd32mp1-debian```
4. ```repo sync```

### Compile docker image
1. ```cd ~/osd32mp1-workspace/docker```
2. ```sudo make build```

### Compile target image for OSD32MP1-RED
1. ```cd ~/osd32mp1-workspace/docker```
2. ```sudo make run```
3. ```make all```

### Outputs
The following outputs are generated in ~/osd32mp1-workspace/deploy :

| Output | Filename | Comment |
| ------ | -------- | ------- |
| 1 | tf-a-stm32mp157c-osd32mp1-red-emmc.stm32 | Binary for OSD32M1 arm-trusted-firmware eMMC |
| 2 | tf-a-stm32mp157c-osd32mp1-red-sdcard.stm32 | Binary for OSD32MP1 arm-trsuted-firmware SD card |
| 3 | tf-a-stm32mp157c-osd32mp1-red-usb.stm32 | Binary for OSD32MP1 arm-trsuted-firmware SD card USB boot |
| 4 | fip-stm32mp157c-osd32mp1-red-trusted.bin | FiP Binary for OSD32MP1 U-Boot |
| 4 | FlashLayout_sdcard_stm32mp157c-osd32mp1-red-trusted.tsv | Flash layout file for OSD32MP1-RED trusted - SD card |
| 5 | FlashLayout_sdcard_stm32mp157c-osd32mp1-red-trusted.raw | Binary image for OSD32MP1-RED trusted - SD card |
| 6 | FlashLayout_emmc_stm32mp157c-osd32mp1-red-trusted.tsv | Flash layout file for OSD32MP1-RED trsudted - eMMC |
| 7 | octavo-bootfs-debian-lxqt-stm32mp157c-osd32mp1-red.ext4 | BOOTFS binary for OSD32MP1-RED |
| 8 | octavo-rootfs-debian-lxqt-emmc-stm32mp157c-osd32mp1-red.ext4 | ROOTFS binary for OSD32MP1-RED - eMMC |
| 9 | octavo-rootfs-debian-lxqt-sdcard-stm32mp157c-osd32mp1-red.ext4 | ROOTFS binary for OSD32MP1-RED - SD card |
| 10 | octavo-vendorfs-debian-lxqt-stm32mp157c-osd32mp1-red.ext4 | VENDORFS binary for OSD32MP1-RED |

### Deploy on SD card (where sdX is the device assignment for SD card)
1. ```cd ~/osd32mp1-workspace/deploy/```
2. ```sudo dd if=FlashLayout_sdcard_stm32mp157c-osd32mp1-red-trusted.raw of=/dev/sdX bs=1M conv=fdatasync status=progress```

### Flash eMMC
STM32MP1 Cube Programmer is required for this procedure

1. Set boot mode for OSD32MP1-RED to USB boot (000)
2. Connect USB-C cable between OSD32MP1-RED and Host computer. 

To determine device numnber for the board connected:

3. ```STM32_Programmer_CLI -l USB```
<pre><font color="#34E2E2"><b>      -------------------------------------------------------------------</b></font>
<font color="#34E2E2"><b>                        STM32CubeProgrammer v2.9.0                  </b></font>
<font color="#34E2E2"><b>      -------------------------------------------------------------------</b></font>

<font color="#4E9A06">=====  DFU Interface   =====</font>

Total number of available STM32 device in DFU mode: 1

  Device Index           : <font color="#34E2E2"><b>USB1</b></font>
  USB Bus Number         : 003
  USB Address Number     : 002
  Product ID             : DFU in HS Mode @Device ID /0x500, @Revision ID /0x0000
  Serial number          : 002700223239511737383434
  Firmware version       : 0x0110
  Device ID              : 0x0500
</pre>

The above device indicates the board is connected as USB1.

Flash the eMMC

4. ```STM32_Programmer_CLI -c port=USB1 -w FlashLayout_emmc_stm32mp157c-osd32mp1-red-trusted.tsv```


## How to contribute
1. Clone repo with a new branch: 
```git checkout https://github.com/octavosystems/xxx -b new_branch_name```
2. Change and test
3. Submit pull request

