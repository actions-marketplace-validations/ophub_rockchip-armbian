# Armbian 构建及使用方法

查看英文说明 | [View English description](README.md)

为了不撰写重复文档，关于 Armbian 的编译及使用等通用方法请查看 [ophub/amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs) 中的文档。这个文档仅记录差异化设置部分，比如特殊的安装方法等。

# 目录

- [Armbian 构建及使用方法](#armbian-构建及使用方法)
- [目录](#目录)
  - [1. 安装 Armbian](#1-安装-armbian)
    - [1.1 Radxa-Rock5B 的安装方法](#11-radxa-rock5b-的安装方法)
      - [1.1.1 安装系统至 microSD](#111-安装系统至-microsd)
      - [1.1.2 安装系统至 eMMC](#112-安装系统至-emmc)
      - [1.1.3 安装系统至 NVMe](#113-安装系统至-nvme)
    - [1.2 电犀牛 R66S 的安装方法](#12-电犀牛-r66s-的安装方法)
    - [1.3 电犀牛 R68S 的安装方法](#13-电犀牛-r68s-的安装方法)
    - [1.4 贝壳云的安装方法](#14-贝壳云的安装方法)
    - [1.5 我家云的安装方法](#15-我家云的安装方法)
  - [2. 更新 Armbian 内核](#2-更新-armbian-内核)
  - [3. 更多使用说明](#3-更多使用说明)

## 1. 安装 Armbian

不同的设备具有不同的存储，有的设备使用外置 microSD 卡，有的带有 eMMC，有的支持使用 NVMe 等多种存储介质，根据设备不同，分别介绍其安装方法。首先在 [Releases](https://github.com/ophub/rockchip-armbian/releases) 里下载自己设备的 Armbian 系统，解压缩成 .img 格式备用。根据自己的设备，使用下面小结中不同的安装方法。

当安装完成后，将 Armbian 设备接入`路由器`，设备开机`2分钟`后，到路由器里查看设备名称为 Armbian 的 `IP`，使用 `SSH` 工具连接进行管理设置。默认用户名为 `root`，默认密码为 `1234`，默认端口为 `22`

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202972715-addcd695-970a-43d6-8a34-24a9c4bc80a2.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202972773-fc9e9ef9-69a7-4279-8329-6fad1cf2f5b9.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202972818-b72e18cd-17d1-4f9f-a0fa-b6c22eef041d.png width="600" />
</div>

### 1.1 Radxa-Rock5B 的安装方法

Radxa-Rock5B 有 microSD/eMMC/NVMe 等多种存储介质可以选择，相应的安装方法也不同。下载 [rk3588_spl_loader_v1.08.111.bin 和 spi_image.img](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/rock5b) 文件备用。下载 [RKDevTool](https://github.com/ophub/kernel/releases/download/tools/Radxa_rock5b_RKDevTool_Release_v2.96__DriverAssitant_v5.1.1.tar.gz) 工具及驱动备用。下载 [Rufus](https://rufus.ie/) 或者 [balenaEtcher](https://www.balena.io/etcher/) 写盘工具备用。

#### 1.1.1 安装系统至 microSD

使用 Rufus 或者 balenaEtcher 等工具将 Armbian 系统镜像写入 microSD 里，然后把写好固件的 microSD 插入设备即可使用。

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202972996-300f223b-f6f6-48af-86ca-bdc842e5017d.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202973216-85b1a21b-0763-4a36-8c57-84490e071fdd.png width="600" />
</div>

#### 1.1.2 安装系统至 eMMC

- 使用 USB 转 eMMC 读卡器安装：将 eMMC 模块与电脑连接，使用 Rufus 或者 balenaEtcher 等工具将 Armbian 系统镜像写入 eMMC 里，然后把写好固件的 eMMC 插入设备即可使用。
- 使用 Maskrom 模式安装：关闭开发板电源。按住金色按钮。将 USB-A 转 C 型电缆插入 ROCK 5B C 型端口，另一端插入 PC。松开金色按钮。检查 USB 设备提示找到一个 MASKROM 设备。右键单击列表的空白区域，然后选择加载 `rock-5b-emmc.cfg` 配置文件（配置文件和 RKDevTool 在同一个目录下）。将 `rk3588_spl_loader_v1.08.111.bin` 和 `Armbian.img` 按下图所示设置，选择写入即可。

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202954823-3d3b1509-eedc-4192-91eb-017269c7f896.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954901-c829d74d-c75a-4fd3-9bd0-aa3cdf2b77b4.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954956-6646b7cf-6354-4dc8-b076-c3323cb89a43.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954998-c08514e2-8760-4c0f-b5f7-0d30be635aa5.png width="600" />
</div>

#### 1.1.3 安装系统至 NVMe

ROCK-5B 在主板上有一个 SPI 闪存，将引导加载程序安装到 SPI 闪存可以支持 SoC maskrom 模式不直接支持的其他启动介质（如 SATA、USB3 或 NVMe）。使用 NVMe 需要先写入 SPI 文件。方法如下：

关闭开发板电源。删除可启动设备，如MicroSD卡，eMMC模块等。按住金色（或某些开发板修订版上的银色）按钮。将 USB-A 转 C 型电缆插入 ROCK-5B C 型端口，另一端插入 PC。松开金色按钮。检查 USB 设备找到一个 MASKROM 设备。在列表框中右键选择加载配置，然后在资源管理文件夹中选择配置文件（配置文件和 RKDevTool 在同一个目录下），根据下图选择 `rk3588_spl_loader_v1.08.111.bin` 和 `spi_image.img` 文件，点击写入即可，如下图所示：

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202954823-3d3b1509-eedc-4192-91eb-017269c7f896.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202961420-8316c96c-2796-43ed-b5ed-2fa5bfa1ddff.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202954956-6646b7cf-6354-4dc8-b076-c3323cb89a43.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202961447-49c0941a-e233-4b2a-b96b-b47636ce3cf2.png width="600" />
</div>

- 使用读卡器安装：将 M.2 NVMe SSD 插入 M.2 NVMe SSD 到 USB3.0 读卡器，以连接到主机。使用 Rufus 或者 balenaEtcher 等工具将 Armbian 系统镜像写入 NVMe 里，然后把写好固件的 NVMe 插入设备即可使用。
- 使用 microSD 卡安装：将 Armbian 系统镜像写入 microSD 卡，将 microSD 卡插入设备并启动，上传 `Armbian.img` 镜像文件到 microSD 卡，使用 `dd` 命令将 Armbian 镜像写入 NVMe 中，命令如下：

```Shell
dd if=armbian.img  of=/dev/nvme0n1  bs=1M status=progress
```

### 1.2 电犀牛 R66S 的安装方法

使用 Rufus 或者 balenaEtcher 等工具将 Armbian 系统镜像写入 microSD 里，然后把写好固件的 microSD 插入设备即可使用。

### 1.3 电犀牛 R68S 的安装方法

- 下载 [RKDevTool](https://github.com/ophub/kernel/releases/download/tools/FastRhino_r68s_RKDevTool_Release_v2.86___DriverAssitant_v5.1.1.tar.gz) 工具及驱动，解压并安装 DriverAssitant 驱动程序，打开 RKDevTool 工具备用。
- R68s 在关机状态下，先插入 USB 双公头线，然后按住 Recovery 键并插上 12V 电源，两秒之后松开 Recovery 键，刷机工具会`发现一个 LOADER 设备`。
- 在 RKDevTool 工具操作界面的空白处点右键，添加项。
- 地址是 `0x00000000`，名字是 `armbian`，路径点击右侧选择 `armbian.img` 系统文件。
- 选择添加的 armbian 一行外，取消其他行的选择，点击执行写入即可。

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202970301-d798677b-e875-4971-ac8f-ee58b2a1e686.png width="200" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970405-cb68cb78-cd0f-43ee-b807-5e594ab73099.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970488-5f18c574-c11f-486f-8fe8-002f3ba2f3f4.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202970577-87549acf-b98b-441f-bb31-e4fd6608108d.png width="600" />
</div>

### 1.4 贝壳云的安装方法

方法转载自 [milton](https://www.cnblogs.com/milton/p/15391525.html) 的教程。刷机需要进入 Maskrom 模式。先断开所有连接，通过短接 CLK 和 GND（使用 TTL 的 GND, 或者旁边小按钮的 GND 均可）这两个触点，然后将 USB 连接到 PC 就会检测到 MASKROM 设备了。短接点位置如下：

<div style="width:100%;margin-top:40px;margin:5px;">
<img src=https://user-images.githubusercontent.com/68696949/202977817-fb12d291-47e2-47e4-88c3-e21f9ae57922.png width="600" /><br />
<img src=https://user-images.githubusercontent.com/68696949/202977900-50b4770d-8444-42a0-8478-3234043455bd.png width="600" />
</div>

打开 RKDevTool 刷机工具，右键添加项。

- 地址 `0xCCCCCCCC`, 名字 `Boot`, 路径[选择](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/beikeyun) `rk3328_loader_v1.14.249.bin`。
- 地址 `0x00000000`, 名字 `system`, 路径选择要刷的 `Armbian.img` 固件。

点击执行写入即可。

### 1.5 我家云的安装方法

方法转载自 [cc747](https://post.smzdm.com/p/a4wkdo7l/) 的教程。刷机需要进入 Maskrom 模式。使我家云处于断电状态，拔掉所有线。用 USB 双公头线，一头插入我家云的 USB2.0 接口，一头插入电脑。用回形针插进 Reset 孔，并按压住不松开。插入电源线。等待几秒钟，直到 RKDevTool 框的下方出现`发现一个LOADER设备`后才松开回形针。将 RKDevTool 切换到`高级功能`点击`进入Maskrom`按钮，提示`发现一个MASKROM设备`。右键添加项。

- 地址 `0xCCCCCCCC`, 名字 `Boot`, 路径[选择](https://github.com/ophub/rockchip-armbian/tree/main/build-armbian/u-boot/l1pro) `rk3328_loader.bin`。
- 地址 `0x00000000`, 名字 `system`, 路径选择要刷的 `Armbian.img` 固件。

点击执行写入即可。


## 2. 更新 Armbian 内核

登录 Armbian 系统 → 输入命令：

```yaml
# 使用 root 用户运行 (sudo -i)
# 如果不指定参数，将更新为最新版本。
armbian-update
```

| 可选参数  | 默认值           | 选项                | 说明               |
| -------  | -------------   | ------------------ | ----------------  |
| -k       | auto latest     | [内核名称](https://github.com/ophub/kernel/tree/main/pub/stable)  | 设置更新内核名称  |
| -v       | stable/rk3588   | stable/rk3588/dev  | 指定内核版本分支     |

举例: `armbian-update -k 5.15.50 -v rk3588`

如果当前目录下有成套的内核文件，将使用当前目录的内核进行更新（更新需要的 4 个内核文件是 `header-xxx.tar.gz`, `boot-xxx.tar.gz`, `dtb-rockchip-xxx.tar.gz`, `modules-xxx.tar.gz`。其他内核文件不需要，如果同时存在也不影响更新，系统可以准确识别需要的内核文件）。如果当前目录没有内核文件，将从服务器查询并下载同系列的最新内核进行更新。在设备支持的可选内核里可以自由更新，如从 5.10.125 内核更新为 5.15.50 内核。

1. `rock5b` 可以选择 [rk3588](https://github.com/ophub/kernel/tree/main/pub/rk3588) 目录下的内核。
2. `电犀牛R66S/R68S` 可以选择 `stable` 目录下的 [6.0.y](https://github.com/ophub/kernel/tree/main/pub/stable) 内核。
3. `贝壳云`/`我家云`可以选择 [stable](https://github.com/ophub/kernel/tree/main/pub/stable) 目录下的全部内核）。

## 3. 更多使用说明

将本地系统中的全部服务脚本更新到最新版本，可以登录 Armbian 系统 → 输入命令：

```yaml
armbian-sync
```

Armbian 固件的本地制作方法，使用 GitHub Actions 云编译的方法，以及更多应用服务的使用方法，在 [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) 项目中有详细说明。在使用 Armbian 系统的过程中，一些常见问题可以查看 [armbian-docs](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs)


