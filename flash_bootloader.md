# Flash SDCARD for STM32MP15XX


## Partition SD card
We want to create 4 partitions as described in STM docs [1] and U-Boot[2]

[1] https://wiki.st.com/stm32mpu/wiki/STM32_MPU_Flash_mapping#SD_card_memory_mapping

[2] https://docs.u-boot.org/en/latest/board/st/stm32mp1.html#prepare-an-sd-card

* Clear existing partitions by zeroing the device
```shell
sudo dd if=/dev/zero of=/dev/your_device bs=1M count=128
#no need to clean the whole device,just the begining 
```
* Create gpt table and 4 partitions. Names and sizes follow convetions in docs [1] and [2]
```shell
sudo part /dev/your_device
(part) mklabel gpt
(part) mkpart fslb1 0% 1MiB
(part) mkpart fslb2 1MiB 2MiB
(part) mkpart fip 2MiB 4MiB
(part) mkpart bootfs 4MiB 68MiB

(parted) print                                                            
Model: Generic- Micro SD/M2 (scsi)
Disk /dev/sdb: 31.0GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name    Flags
 1      17.4kB  1049kB  1031kB               fsbl1
 2      1049kB  2097kB  1049kB               fsbl2
 3      2097kB  4194kB  2097kB               fip
 4      4194kB  71.3MB  67.1MB  ext4         bootfs
```


Note: Use sectors as units, since MiB and MB will generate alignment issues.

* Now, format the boot partition as an ext4 filesystem. This is where U-Boot saves its environment:
```shell
$ sudo mkfs.ext4 -L boot -O ^metadata_csum /dev/mmcblk0p4
```
The `-O ^metadata_csum` option allows to create the filesystem without enabling metadata checksums, which
U-Boot doesn’t seem to support yet


## Write the relevant binaries

* fsbl (BL2) and fsbl2 (backup for BL2) include TF-A
```shell
sudo dd of=/dev/sdb1 if=bootloader/trusted-firmware-a/build/stm32mp1/release/tf-a-stm32mp157a-dk1.stm32 bs=1M conv=fdatasync
```

```shell
sudo dd of=/dev/sdb2 if=bootloader/trusted-firmware-a/build/stm32mp1/release/tf-a-stm32mp157a-dk1.stm32 bs=1M conv=fdatasync
```


* The `fip` is located in partition 3 or a partition named fip check relevant docs for each target
```shell
sudo dd of=/dev/sdb3 if=bootloader/trusted-firmware-a/build/stm32mp1/release/fip.bin  bs=1M conv=fdatasync
```