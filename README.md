ENGICAM i.Core M6 Board Support for Linux
=========================================

See https://github.com/engicam-stable/ for the official code.


Building Linux
--------------

```
make uImage LOADADDR=15000000
make imx6q-engicam.dtb
```

Booting u-boot via USB
----------------------

  * close JM2 on Board
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

Original u-boot output
----------------------

```
U-Boot 2009.08-dirty (Apr 16 2014 - 15:55:46)
CPU: Freescale i.MX6 family TO1.2 at 792 MHz
Thermal sensor with ratio = 185
Temperature:   44 C, calibration data 0x5904f47
mx6q pll1: 792MHz
mx6q pll2: 528MHz
mx6q pll3: 480MHz
mx6q pll8: 50MHz
ipg clock     : 66000000Hz
ipg per clock : 66000000Hz
uart clock    : 80000000Hz
cspi clock    : 60000000Hz
ahb clock     : 132000000Hz
axi clock   : 264000000Hz
emi_slow clock: 132000000Hz
ddr clock     : 528000000Hz
usdhc1 clock  : 198000000Hz
usdhc2 clock  : 198000000Hz
usdhc3 clock  : 198000000Hz
usdhc4 clock  : 198000000Hz
nfc clock     : 19800000Hz
Board: MX6Q-i.Core:[ POR]
Boot Device: NAND
I2C:   ready
DRAM:   1 GB
NAND:  ONFI param page 0 valid
ONFI flash detected
Manufacturer ID: 0x2c, Chip ID: 0xaa (Micron MT29F2G08ABBEAH4), page size: 2048, OOB size: 64
256 MiB
MMC:   FSL_ESDHC: 0,FSL_ESDHC: 1
In:    serial
Out:   serial
Err:   serial
Net:   CCGR1 = 0x30FC00
ANALOG_PLL_ENET = 0x11001
ANALOG_PLL_ENET = 0x2001
FEC0 [PRIME]
Version: Engicam U-Boot 1.10
Note:    iCore6Q default U-Boot
Hit any key to stop autoboot:
MX6Q i.Core U-Boot > showvar
HUSH_VERSION=0.01
MX6Q i.Core U-Boot > printenv
bootdelay=3
baudrate=115200
ipaddr=192.168.2.75
serverip=192.168.2.96
netmask=255.255.255.0
loadaddr=0x10800000
rd_loadaddr=0x11000000
netdev=eth0
ethprime=FEC0
nfsroot=nfs_icore
bootargs_net=setenv bootargs ${bootargs} root=/dev/nfs ip=dhcp nfsroot=${serverip}:${nfsroot},v3,tcp
bootargs_mmc=setenv bootargs ${bootargs} root=/dev/mmcblk0p1 rootwait rw
bootargs_ubi=setenv bootargs ${bootargs} ubi.mtd=2 root=ubi0:rootfs rootfstype=ubifs rootwait rw
erase=nand erase 1c0000 4000
ethact=FEC0
GEA_SN=E04178
fec_addr=XX:XX:XX:XX:XX:XX
fecaddr=XX:XX:XX:XX:XX:XX
ethaddr=XX:XX:XX:XX:XX:XX
board=SK.RES
video_type=mxcfb0:dev=lcd
lcd_panel=Amp-WD
bootcmd=run bootcmd_mmc; run bootcmd_ubi
bootcmd_mmc=run set_bootargs; run bootargs_mmc; mmc dev 0; mmc read 0x10800000 0x800 0x2000;bootm
bootcmd_net=run set_bootargs; run bootargs_net; tftp uImage; bootm
bootcmd_ubi=run set_bootargs; run bootargs_ubi; nand read 0x10800000 0x400000 0x700000;bootm
bootargs=noinitrd console=ttymxc3,115200 arm_freq=800 engi_board=SK.RES video=mxcfb0:dev=lcd,Amp-WD fec_mac=XX:XX:XX:XX:XX:XX
set_bootargs=set bootargs noinitrd console=ttymxc3,115200 arm_freq=800 engi_board=${board} video=${video_type},${lcd_panel} fec_mac=XX:XX:XX:XX:XX:XX
stdin=serial
stdout=serial
stderr=serial
Environment size: 1256/131068 bytes
```

Evaluation Board JTAG to FTDI UM232H-B
--------------------------------------

```
ESP JTAG J7		FTDI UM232H-B
 1 +JTAG_PWR	-	nu
 2 JTAG_TRSTB	-	C0	5 top
 3 JTAG_TDI	-	D1	1 bottom
 4 JTAG_TMS	-	D3	2 bottom
 5 JTAG_TCK	-	D0	1 top
 6 GND		-	GND	9 bottom
 7 nc
 8 JTAG_TDO	-	D2	2 top
 9 nc
10 JTAG_nSRST	-	C1	5 bottom
```

Set J4 JTAGPWR 1-2 3,3V!


Assorted links
--------------

Linaro on Freescale i.MX6Q Sabre Lite Board
  https://wiki.linaro.org/Boards/MX6QSabreLite

Linux on ARM  Home i.MX6x SABRE Lite
  http://eewiki.net/display/linuxonarm/i.MX6x+SABRE+Lite

Linux Fast Boot on i.MX6 Sabresd Board
  https://community.freescale.com/docs/DOC-93619

How to change the boot screen of Linux 2.6.35 kernel ?
  https://community.freescale.com/thread/304300

http://boundarydevices.com/u-boot-on-i-mx6/

UBIFS: http://www.linux-mtd.infradead.org/doc/ubifs.html

