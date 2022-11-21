# Armbian for Rockchip / 瑞芯微·岸边

查看英文说明 | [View English description](README.md)

当前支持 `瑞莎 Rock5b`，`电犀牛R66S/R68S`， `贝壳云`，`我家云` 等设备，使用 [unifreq's](https://github.com/unifreq) 的加强版 bootloader 和最新版本的内核进行了重制。添加了在 [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) 项目中开发的更多应用和服务，支持写入 `TF/USB/eMMC/NVME` 中使用。

最新版固件可以在 [Releases](https://github.com/ophub/rockchip-armbian/releases) 中下载。

## Armbian 固件说明

| 芯片  | 设备 | [可选内核](https://github.com/ophub/kernel/tree/main/pub) | Armbian 固件 |
| ---- | ---- | ---- | ---- |
| rk3588 | [Radxa-Rock5B](https://wiki.radxa.com/Rock5/5b) | [rk3588](https://github.com/ophub/kernel/tree/main/pub/rk3588) | armbian_rockchip_rock5b.img |
| rk3568 | [FastRhino-R66S](https://r68s.cn/), [FastRhino-R68S](https://r68s.cn/) | [6.0.y](https://github.com/ophub/kernel/tree/main/pub/stable) | armbian_rockchip_r66s.img <br />armbian_rockchip_r68s.img |
| rk3328 | [beikeyun](https://www.cnblogs.com/milton/p/15391525.html), [l1pro](https://post.smzdm.com/p/a4wkdo7l/) | [stable](https://github.com/ophub/kernel/tree/main/pub/stable) | armbian_rockchip_beikeyun.img <br />armbian_rockchip_l1pro.img |

💡提示：在下载列表中查找与设备名称匹配的固件，如 Radxa-Rock5B 的固件是 Armbian_x_rock5b_x.img.gz

## 安装和更新方法

使用 [Rufus](https://rufus.ie/) 或者 [balenaEtcher](https://www.balena.io/etcher/) 等工具将固件写入 TF/USB 里，然后把写好固件的 TF/USB 插入设备。

- ### 安装 Armbian

不同设备的安装方法不同，请查看[说明文档](build-armbian/armbian-docs/)。

- ### 更新 Armbian 内核

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

- ### 更多使用说明

将本地系统中的全部服务脚本更新到最新版本，可以登录 Armbian 系统 → 输入命令：

```yaml
armbian-sync
```

Armbian 固件的本地制作方法，使用 GitHub Actions 云编译的方法，以及更多应用服务的使用方法，在 [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) 项目中有详细说明。在使用 Armbian 系统的过程中，一些常见问题可以查看 [armbian-docs](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs)

## Armbian 固件默认信息

| 名称 | 值 |
| ---- | ---- |
| 默认 IP | 从路由器获取 IP |
| 默认账号 | root |
| 默认密码 | 1234 |

## 使用 GitHub Actions 编译内核

内核的编译方法详见 [compile-kernel](.github/workflows/compile-kernel.yml)，其中 rk3588 系列详见 [compile-kernel-rk3588](.github/workflows/compile-kernel-rk3588.yml)。这 2 个系列的内核不通用。

```yaml
- name: Compile the kernel
  uses: ophub/amlogic-s9xxx-armbian@main
  with:
    build_target: kernel
    kernel_version: 5.10.150_5.15.75
    kernel_auto: true
    kernel_sign: -yourname
```

## 其他发行版

- [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) 项目提供了在晶晨系列盒子里使用的 `Armbian` 系统，欢迎使用。
- [amlogic-s9xxx-openwrt](https://github.com/ophub/amlogic-s9xxx-openwrt) 项目提供了在晶晨系列盒子里使用的 `OpenWrt` 系统，欢迎使用。
- [unifreq](https://github.com/unifreq/openwrt_packit) 为晶晨、瑞芯微和全志等更多盒子制作了 `OpenWrt` 系统，属于盒子圈的标杆，推荐使用。

## 链接

- [armbian](https://github.com/armbian/build)
- [Armbian for Amlogic](https://github.com/ophub/amlogic-s9xxx-armbian)
- [unifreq](https://github.com/unifreq)
- [kernel.org](https://kernel.org)

## License

The rockchip-armbian © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/rockchip-armbian/blob/main/LICENSE)

