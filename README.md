# Armbian for Rockchip

View Chinese description  |  [查看中文说明](README.cn.md)

Currently `Radxa 5B`, `beikeyun`, `l1pro` and other devices are supported, using [unifreq's](https://github.com/unifreq) enhanced version of bootloader and the latest kernel, More applications and services in [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) have been added, which can be written to `TF/USB/eMMC/NVME` for use.

The latest version of the Armbian firmware can be downloaded in [Releases](https://github.com/ophub/rockchip-armbian/releases).

## Armbian Firmware instructions

| SoC  | Device | [Optional kernel](https://github.com/ophub/kernel/tree/main/pub) | Armbian Firmware |
| ---- | ---- | ---- | ---- |
| rk3588 | [Radxa-Rock5B](https://wiki.radxa.com/Rock5/5b) | [rk3588](https://github.com/ophub/kernel/tree/main/pub/rk3588) | armbian_rockchip_rock5b.img |
| rk3328 | [beikeyun](https://www.cnblogs.com/milton/p/15391525.html), [l1pro](https://post.smzdm.com/p/a4wkdo7l/) | [stable](https://github.com/ophub/kernel/tree/main/pub/stable) | armbian_rockchip_beikeyun.img <br />armbian_rockchip_l1pro.img |

💡Tip: Find the firmware matching the device name in the download list. For example, the firmware of Radxa-Rock5B is Armian_x_rock5b_x.img.gz

## Install and update instructions

Write the IMG file to the USB/TF hard disk through software such as [Rufus](https://rufus.ie/) or [balenaEtcher](https://www.balena.io/etcher/). Insert the USB/TF hard disk into the device.

- ### Install Armbian

Login in to armbian (default user: root, default password: 1234) → Upload the Armbian image → input command:

```yaml
dd if=armbian.img  of=/dev/<your_device_name>  bs=1M conv=fsync

# For example, the write command in NVME is:
# dd if=armbian.img  of=/dev/nvme0n1  bs=1M conv=fsync
```

💡Tip: If `Radxa 5B` uses the Armbian system in `NVME` or `USB`, you must use [the spi bootloader files downloaded here](build-armbian/u-boot/rock5b). Please refer to the official website for [the brushing method](https://wiki.radxa.com/Rock5/install/spi).


- ### Update Armbian Kernel

Login in to armbian → input command:

```yaml
# Run as root user (sudo -i)
# If no parameter is specified, it will update to the latest version.
armbian-update
```

If there is a set of kernel files in the current directory, it will be updated with the kernel in the current directory (The 4 kernel files required for the update are `header-xxx.tar.gz`, `boot-xxx.tar.gz`, `dtb-rockchip-xxx.tar.gz`, `modules-xxx.tar.gz`. Other kernel files are not required. If they exist at the same time, it will not affect the update. The system can accurately identify the required kernel files). If there is no kernel file in the current directory, it will query and download the latest kernel of the same series from the server for update. You can also query the [Optional Kernel](https://github.com/ophub/kernel/tree/main/pub) version (The kernels of rk3588 and other series are not common and must be used separately. For example, rock5b can select the kernels in the rk3588 directory. beikeyun/l1pro can select the kernels in the stable directory.) and update the specified version: `armbian-update 5.10.150 stable`. The optional kernel supported by the device can be freely updated, such as from 5.10.150 kernel to 5.15.75 kernel.

- ### More instructions for use

To update all service scripts in the local system to the latest version, you can login in to armbian → input command:

```yaml
armbian-sync
```

The local production method of Armbian firmware, the method of using GitHub Actions cloud compilation, and the method of using more application services are described in detail in project [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian). In the process of using the Armbian system, some common problems can be found in [armbian-docs](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/build-armbian/armbian-docs).


## Armbian firmware default information

| Name | Value |
| ---- | ---- |
| Default IP | Get IP from the router |
| Default username | root |
| Default password | 1234 |

## Compile the kernel using GitHub Actions

See [compile-kernel](.github/workflows/compile-kernel.yml) for the kernel compilation method, and [compile-kernel-rk3588](.github/workflows/compile-kernel-rk3588.yml) for the rk3588 series. The two series of kernels are not common.

```yaml
- name: Compile the kernel
  uses: ophub/amlogic-s9xxx-armbian@main
  with:
    build_target: kernel
    kernel_version: 5.10.150_5.15.75
    kernel_auto: true
    kernel_sign: -yourname
```

## Other distributions

- The [amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian) project provides the `Armbian` system for `Amlogic` boxes. Welcome to use it.
- The [amlogic-s9xxx-openwrt](https://github.com/ophub/amlogic-s9xxx-openwrt) project provides the `OpenWrt` system for `Amlogic` boxes. Welcome to use it.
- [unifreq](https://github.com/unifreq/openwrt_packit) has made `OpenWrt` system for more boxes such as `Amlogic`, `Rockchip` and `Allwinner`, which is a benchmark in the box circle and is recommended for use.

## Links

- [armbian](https://github.com/armbian/build)
- [Armbian for Amlogic](https://github.com/ophub/amlogic-s9xxx-armbian)
- [unifreq](https://github.com/unifreq)
- [kernel.org](https://kernel.org)

## License

The rockchip-armbian © OPHUB is licensed under [GPL-2.0](https://github.com/ophub/rockchip-armbian/blob/main/LICENSE)

