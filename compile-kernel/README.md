# Kernel compilation and usage instructions / 内核编译和使用说明

See [compile-kernel](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/compile-kernel) for detailed parameter descriptions of kernel compilation. The kernel of rk3588 and other series are not common and must be used separately. For example, rock5b can choose the kernel of rk3588 series. beikeyun/l1pro can choose the stable series kernel. The source code used to compile the kernels of these two series is also different. Parameters must be specified separately during compilation, as shown below:

内核编译的详细参数说明查看 [compile-kernel](https://github.com/ophub/amlogic-s9xxx-armbian/tree/main/compile-kernel)。其中 rk3588 和其他系列的内核不通用，须区分使用。如 rock5b 可以选择 rk3588 系列的内核。贝壳云/我家云 可以选择 stable 系列的内核。编译这两种系列的内核使用的源码也不同，编译时须分别指定参数，举例如下：

## Compile the kernel of the stable family / 编译 stable 系列的内核

Refer to the template [compile-kernel](../.github/workflows/compile-kernel.yml)

可以参考模板 [compile-kernel](../.github/workflows/compile-kernel.yml)

```yaml
- name: Compile the kernel
  uses: ophub/amlogic-s9xxx-armbian@main
  with:
    build_target: kernel
    kernel_version: 5.10.125_5.15.50
    kernel_auto: true
    kernel_sign: -yourname
```

## Compile the kernel of the rk3588 family / 编译 rk3588 系列的内核

Refer to the template [compile-kernel-rk3588](../.github/workflows/compile-kernel-rk3588.yml)

可以参考模板 [compile-kernel-rk3588](../.github/workflows/compile-kernel-rk3588.yml)

```yaml
- name: Compile the kernel
  uses: ophub/amlogic-s9xxx-armbian@main
  with:
    build_target: kernel
    kernel_repo: unifreq/linux-rock5b
    kernel_version: 5.10.125_5.15.50
    kernel_auto: true
    kernel_config: compile-kernel/config
    kernel_sign: -yourname
```
