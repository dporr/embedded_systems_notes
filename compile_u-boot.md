# Compile U-Boot

Docs [1] https://docs.u-boot.org/en/latest/board/st/index.html

* Chose a config from the supported boards under `configs/`, in our case `configs/stm32mp15_defconfig`
```shell
make stm32mp15_defconfig
```

* optional customize u-boot features
```shell
make menuconfig
```

* chose a device tree from `dts/upstream/src/<ARCH>/<VENDOR>/<BOARD>` in our case `stm32mp157a-dk1`
```shell
make DEVICE_TREE=stm32mp157a-dk1 -j $(nproc)
```