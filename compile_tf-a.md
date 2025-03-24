# Compiling TF-A
From TF-A documentation for STM32MP1 [1]

```
make ARM_ARCH_MAJOR=7 ARCH=aarch32 PLAT=stm32mp1 AARCH32_SP=sp_min \
DTB_FILE_NAME=stm32mp157a-dk1.dtb BL33=../u-boot/u-boot-nodtb.bin \
BL33_CFG=../u-boot/u-boot.dtb STM32MP_SDMMC=1 fip all
```

* Making sense of the parameters:

`ARM_ARCH_MAJOR` and `ARCH` depend on the target device. STM32157C devices thhave a Cortex-A7[3] wich is an aarc32 core compatible with ARMv7-A

`PLAT` A subfolder under `plat/` in our case  `stm32mp1` 

`BL33` This is the no-secure world bootloader/u-boot in our case

`BL33_CFG` Device tree for u-boot (since the device tree TF-A is not visible to u-boot running from RAM)

`DTB_FILE_NAME` device tree under `fdts/`

`AARCH32_SP` Choose the AArch32 Secure Payload component to be built as as the BL32 image when ARCH=aarch32. The value should be the path to the directory containing the SP source, relative to the bl32/; the directory is expected to contain a makefile called <aarch32_sp-value>.mk.

[1]https://trustedfirmware-a.readthedocs.io/en/latest/plat/st/index.html

[2] https://www.st.com/en/microcontrollers-microprocessors/stm32mp157.html

[3] https://en.wikipedia.org/wiki/ARM_Cortex-A7