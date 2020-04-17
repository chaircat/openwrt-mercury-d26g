# Unofficial OpenWRT Support for Mercury D26G Wireless AC Router

Router Specification
--------------------

```
SoC: MediaTek MT7621A
RAM: DDR3 128 MiB
Flash Memory: 8 MiB
Wi-Fi 2.4G: MediaTek MT7615
Wi-Fi 5G: MediaTek MT7615
Ethernet: 4x 10 Mbps / 100 Mbps / 1000 Mbps (1x WAN, 3x LAN)
LED: 1x Internal White LED to Blue LED Cover
Input: 1x tactile switch (Reset)
Serial console: 57600bps, PCB through hole ( TX [R19], RX [R21,R24] , GND, Vcc ) Vcc does not need to be connected
Power: DC 12V
```

Notes
-----
```
Mercury D26G uses RSA signed bootloader hence it is almost impossible to install OpenWRT withoout replacing the bootloader
This also means official openwrt support will most likely never happen
The suitable bootloader for this is breed-mt7621-gehua-ghl-r-001.bin and breed-mt7621-creativebox-v1.bin ( inside uboot-replacement directory )
Both bootloader does not have GPIO conflict with the reset button 
breed-mt7621-gehua-ghl-r-001.bin operates at baudrate of 57600bps but slightly unstable (boot hangs)
breed-mt7621-creativebox-v1.bin operates at a different baudrate hence serial is messed up but is much more stable (no boot hangs)
It also allows easy overclocking and firmware recovery
However the bootloader reflash functionality should not be used as it will overwrite some board information
The SPI flash cannot be programmed without plugging in the power as the pins need to be driven so the flash programmer can write data to the chip
As of now everything works except Wireless AC 5GHZ and PCIe ocassional hang issues due to Openwrt MT7621 Kernel 5.4 due to driver unavailability 
```

Installation
------------

```
A serial SPI flash reprogrammer is required
A serial to usb converter to connect to the Serial Console is recommend to know what is happening

Step 1: Connect up the external SPI flash Programmer to the SPI Flash Chip
Step 2: Plug in the AC power into the router, the router will not boot and that is normal
Step 3: Launch the external SPI Flash Progammer software and read the entire chip and save a full backup of the flash image
Step 4: Use a HexEditor like HxD, copy the breed bootloader and paste write the contents into the flash image from offset 0, do not modify the set of the flash image
Step 5: Save the modified image and perform a SPI chip erase, after that open the modified image and program the contents into the SPI Flash Chip
Step 6: Build Openwrt from the commits of this repository
Step 7: Unplug the flash programmer and reboot the router 
Step 8: Go to 192.168.1.1 where the breed recovery page is present and upload the openwrt sysupgrade file into the breed bootloader
Step 9: Reboot

The router is now running openwrt
```

This is the buildsystem for the OpenWrt Linux distribution.

To build your own firmware you need a Linux, BSD or MacOSX system (case
sensitive filesystem required). Cygwin is unsupported because of the lack
of a case sensitive file system.

You need gcc, binutils, bzip2, flex, python3.5+, perl, make, find, grep, diff,
unzip, gawk, getopt, subversion, libz-dev and libc headers installed.

1. Run "./scripts/feeds update -a" to obtain all the latest package definitions
defined in feeds.conf / feeds.conf.default

2. Run "./scripts/feeds install -a" to install symlinks for all obtained
packages into package/feeds/

3. Run "make menuconfig" to select your preferred configuration for the
toolchain, target system & firmware packages.

4. Run "make" to build your firmware. This will download all sources, build
the cross-compile toolchain and then cross-compile the Linux kernel & all
chosen applications for your target system.

Sunshine!
	Your OpenWrt Community
	http://www.openwrt.org


