Building Linux
--------------

```
make uImage LOADADDR=15000000
make imx6q-engicam.dtb
```

Booting u-boot via USB
----------------------

  * close JM32 on Board
  * `sudo ./imx_usb_loader/imx_usb uboot-imx/u-boot.imx`

u-boot on SD
------------

```
dd if=u-boot.imx оf=/dev/sdX bs=1024 seek=1 && sync
```

Booting Linux from u-boot
-------------------------

```
setenv ethaddr 00:00:00:ff:fa:11; setenv bootargs console=ttymxc3,115200 root=/dev/nfs rw ip=dhcp nfsroot=172.16.172.100:/srv/export/nfsroots/icorem6,v3,tcp; dhcp 0x11000000 172.16.172.100:imx6q-engicam.dtb;dhcp 0x12000000 172.16.172.100:uImage;bootm 0x12000000 - 0x11000000
```

