# How to build and use Armbian

View Chinese description  |  [查看中文说明](README.cn.md)

In order not to write duplicate documents, please refer to the documents in [ophub/amlogic-s9xxx-armian](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs) for general methods such as compiling and using Armbian. This document only records the differential settings, such as special installation methods.

# Tutorial directory

- [How to build and use Armbian](#how-to-build-and-use-armbian)
- [Tutorial directory](#tutorial-directory)
  - [1. Install Armbian](#1-install-armbian)
    - [1.1 Installation method of Radxa-Rock5B](#11-installation-method-of-radxa-rock5b)
      - [1.1.1 Installing the system to microSD](#111-installing-the-system-to-microsd)
      - [1.1.2 Installing the system to eMMC](#112-installing-the-system-to-emmc)
      - [1.1.3 Installing the system to NVMe](#113-installing-the-system-to-nvme)
    - [1.2 Installation method of FastRhino-R66S](#12-installation-method-of-fastrhino-r66s)
    - [1.3 Installation method of FastRhino-R68S](#13-installation-method-of-fastrhino-r68s)
    - [1.4 Installation method of BeikeYun](#14-installation-method-of-beikeyun)
    - [1.5 Installation method of l1pro](#15-installation-method-of-l1pro)
  - [2. Update Armbian Kernel](#2-update-armbian-kernel)
  - [3. More instructions for use](#3-more-instructions-for-use)


## 1. Install Armbian

Different devices have different storage. Some devices use external TF cards, some have eMMC, and some support the use of NVMe and other storage media. According to different devices, their installation methods are introduced respectively. Use the different installation methods in the following summary according to your own devices.

When the installation is complete, Connect the Armbian device to the `router`. After the device is powered on for `2 minutes`, check the `IP` address of the device named `Armbian` in the router, and use the `SSH` tool to connect for management settings. The default user name is `root`, the default password is `1234`, and the default port is `22`

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202972715-addcd695-970a-43d6-8a34-24a9c4bc80a2.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202972773-fc9e9ef9-69a7-4279-8329-6fad1cf2f5b9.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202972818-b72e18cd-17d1-4f9f-a0fa-b6c22eef041d.png width="600" />
</div>

### 1.1 Installation method of Radxa-Rock5B

Radxa-Rock5B has a variety of storage media to choose from, including microSD/eMMC/NVMe, and the corresponding installation methods are different. Download [rk3588_spl_loader_v1.08.111.bin and spi_image.img files](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/rock5b) for backup. Download [RKDevTool](https://github.com/ophub/kernel/releases/download/tools/Radxa_rock5b_RKDevTool_Release_v2.96__DriverAssitant_v5.1.1.tar.gz) tool and drive backup. Download [Rufus](https://rufus.ie/) Or [balenaEtcher](https://www.balena.io/etcher/) disk writing tools for backup.

#### 1.1.1 Installing the system to microSD

Using Rufus or balenaEtcher and other tools to write the Armbian system image into the microSD, and then insert the microSD with the firmware into the device for use.

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202972996-300f223b-f6f6-48af-86ca-bdc842e5017d.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202973216-85b1a21b-0763-4a36-8c57-84490e071fdd.png width="600" />
</div>

#### 1.1.2 Installing the system to eMMC

- Install using USB to eMMC card reader: connect the eMMC module to the computer, and use Rufus Or balenaEtcher and other tools to write the image of the Armbian system into the eMMC, and then insert the eMMC with the firmware into the device for use.
- Install using Maskrom mode: Power off the board. Press the golden button and hold it. Plug the USB-A to Type-C cable to ROCK 5B Type-C port, the other side to PC. Release the golded button. Check usb device is in Found One MASKROM Device. Right-click in the blank area of the list and select Load configuration file(The configuration file and RKDevTool are in the same directory). Select `rk3588_spl_loader_v1.08.111.bin` and `Armbian.img` as shown in the figure below and write them in.

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202954823-3d3b1509-eedc-4192-91eb-017269c7f896.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954901-c829d74d-c75a-4fd3-9bd0-aa3cdf2b77b4.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954956-6646b7cf-6354-4dc8-b076-c3323cb89a43.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954998-c08514e2-8760-4c0f-b5f7-0d30be635aa5.png width="600" />
</div>

#### 1.1.3 Installing the system to NVMe

ROCK-5B has one SPI Flash on the board, it's useful to install the bootloader to the SPI flash for a back up booting and support other booting media that the SoC maskrom mode doesn't direct support, such as SATA, USB 3 or NVMe. Using NVMe requires writing SPI files first. The method is as follows:

Power off the board. Remove bootable device like MicroSD card, eMMC module, etc. Press the golden (or silver on some board revisions) button and hold it. Plug the USB-A to Type-C cable to ROCK-5B Type-C port, the other side to PC. Release the golded button. Check usb device is Found One MASKROM Device. Right-click in the list box and select Load Config,Then select the configuration file in the resource management folder（The configuration file and RKDevTool are in the same directory）, click the right last columns in the `Loader` row to select `rk3588_spl_loader_v1.08.111.bin`, click the right last columns in the `spi` row to select `spi_image.img`. Finally, click the `Run` button, and you will see the content in the red box on the right. When the progress reaches 100%, the download is completed. As shown in the figure below:

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202954823-3d3b1509-eedc-4192-91eb-017269c7f896.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202961420-8316c96c-2796-43ed-b5ed-2fa5bfa1ddff.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954956-6646b7cf-6354-4dc8-b076-c3323cb89a43.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202961447-49c0941a-e233-4b2a-b96b-b47636ce3cf2.png width="600" />
</div>

- Install with a card reader: insert the M.2 NVMe SSD into the M.2 NVMe SSD to the USB3.0 card reader to connect to the host. Use tools such as Rufus or balenaEtcher to write the Armbian system image to NVMe, and then insert the NVMe with the firmware into the device for use.
- Installation with microSD card: write the image of the Armbian system into the microSD card, insert the microSD card into the device, start it, and upload the Armbian image file is to the microSD card, and then use the `dd` command to write the `Armbian.img` to NVMe. The command is as follows:

```Shell
dd if=armbian.img  of=/dev/nvme0n1  bs=1M status=progress
```

### 1.2 Installation method of FastRhino-R66S

Use tools such as Rufus or balenaEtcher to write the Armbian system image into the microSD, and then insert the microSD with the firmware into the device for use.

### 1.3 Installation method of FastRhino-R68S

- Download the [RKDevTool](https://github.com/ophub/kernel/releases/download/tools/FastRhino_r68s_RKDevTool_Release_v2.86___DriverAssitant_v5.1.1.tar.gz) tool and driver, decompress and install the DriverAssistant driver, and open the RKDevTool tool for standby.
- When the R68s is powered off, first insert the USB dual male cable, then press and hold the Recovery key and plug in the 12V power supply. After two seconds, release the Recovery key, and the brushing tool will `find a LOADER device`.
- Right click the blank space of the RKDevTool tool operation interface to add an item.
- The address is `0x00000000` and the name is `armbian`. Click the path on the right to select `armbian.img` system files.
- Select an additional armbian line, deselect other lines, and click `Run` Write.

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202970301-d798677b-e875-4971-ac8f-ee58b2a1e686.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970405-cb68cb78-cd0f-43ee-b807-5e594ab73099.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970488-5f18c574-c11f-486f-8fe8-002f3ba2f3f4.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970577-87549acf-b98b-441f-bb31-e4fd6608108d.png width="600" />
</div>

### 1.4 Installation method of BeikeYun

Method reproduced from milton's [tutorial](https://www.cnblogs.com/milton/p/15391525.html). Flashing needs to enter Maskrom mode. Disconnect all connections first, short the two contacts of CLK and GND (use the GND of TTL, or the GND of the small button next to it), and then connect the USB to the PC to detect the MASKROM device. The position of the short-circuit point is as follows:

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202977817-fb12d291-47e2-47e4-88c3-e21f9ae57922.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202977900-50b4770d-8444-42a0-8478-3234043455bd.png width="600" />
</div>

Open the RKDevTool flashing tool, right-click to add an item.

- Address `0xCCCCCCCC`, name `Boot`, path [select](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/beikeyun) `rk3328_loader_v1.14.249.bin`.
- Address `0x00000000`, name `system`, path select `Armbian.img` firmware.

Click to execute the write.


### 1.5 Installation method of l1pro

Method reproduced from cc747's [tutorial](https://post.smzdm.com/p/a4wkdo7l/). Flashing needs to enter Maskrom mode. Make l1pro power off and unplug all cables. Use a USB double-male cable, plug one end into the USB2.0 port of l1pro, and plug the other end into the computer. Insert a paper clip into the Reset hole and press it down firmly. Plug in the power cord. Wait for a few seconds until `Found a LOADER device` appears at the bottom of the RKDevTool box before releasing the paperclip. Switch RKDevTool to `Advanced Functions` and click the `Enter Maskrom` button, prompting `Found a MASKROM device`.

Right click to add item.

- Address `0xCCCCCCCC`, name `Boot`, path [select](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/l1pro) `rk3328_loader.bin`.
- Address `0x00000000`, name `system`, path to select the `Armbian.img` firmware to flash.

Click to execute the write.

## 2. Update Armbian Kernel

Login in to armbian → input command:

```yaml
# Run as root user (sudo -i)
# If no parameter is specified, it will update to the latest version.
armbian-update
```

| Optional  | Default         | Value              | Description                   |
| -------   | -------------   | -----------------  | ---------------------------   |
| -k        | auto latest     | [kernel name](https://github.com/ophub/kernel/tree/main/pub/stable)  | Set the kernel name |
| -v        | stable/rk3588   | stable/rk3588/dev  | Set the kernel version branch |

Example: `armbian-update -k 5.15.50 -v rk3588`

If there is a set of kernel files in the current directory, it will be updated with the kernel in the current directory (The 4 kernel files required for the update are `header-xxx.tar.gz`, `boot-xxx.tar.gz`, `dtb-rockchip-xxx.tar.gz`, `modules-xxx.tar.gz`. Other kernel files are not required. If they exist at the same time, it will not affect the update. The system can accurately identify the required kernel files). If there is no kernel file in the current directory, it will query and download the latest kernel of the same series from the server for update. The optional kernel supported by the device can be freely updated, such as from 5.10.125 kernel to 5.15.50 kernel.

1. `rock5b` can select the kernels in the [rk3588](https://github.com/ophub/kernel/tree/main/pub/rk3588) directory.
2. `FastRhino-R66S/R68S` can choose the [6.0.y](https://github.com/ophub/kernel/tree/main/pub/stable) kernel in the `stable` directory
3. `beikeyun/l1pro` can select the all kernels in the [stable](https://github.com/ophub/kernel/tree/main/pub/stable) directory.

## 3. More instructions for use

To update all service scripts in the local system to the latest version, you can login in to armbian → input command:

```yaml
armbian-sync
```

The local production method of Armbian firmware, the method of using GitHub Actions cloud compilation, and the method of using more application services are described in detail in project [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian). In the process of using the Armbian system, some common problems can be found in [armbian-docs](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs).

