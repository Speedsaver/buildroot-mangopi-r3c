# External MangoPi R3C buildroot tree #

This external tree contains the support for Widora MangoPi R3C revision only. This is the vendor made configs and board scripts, with little to no modifications.

* Default nand image configuration was changed from using 120M to 128M.
* Gigadevice spi flash support was enabled in u-boot to recognize the gigadevice spinand onboard.
* Compiler cache feature is enabled to provide faster compilation times with future builds.
* An external toolchain from bootlin is used so that the build doesn't have to build its own toolchain to compile programs.
* Per package directory is enabled to support top-level parallel building.

## How to build ##

Tested working with Buildroot 2023.02.2. Note login is root, and no password is required

Download buildroot, then git clone this external tree, cd to the buildroot top level directory, then type these commands:

```
make BR2_EXTERNAL=/path/to/buildroot-mangopi-r3c widora_mangopi_r3_defconfig O=output/widora-mangopi-r3
```
```
cd output/widora-mangopi-r3
```
```
make
```

If you have a multi-cores machine, you can try passing -j to the second make invocation to accelerate the build.

## Current problems ##

Currently, usb DFU is totally broken, so it is impossible to flash the nand flash ("## Error: "dfu_nand" not defined" when trying to run dfu_nand)

Micro sd card boot doesn't seem to boot up on its own and you need to pass these commands to boot up:

```
load mmc 0:2 ${kernel_addr_r} kernel.itb
bootm ${kernel_addr_r}
```
