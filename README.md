## Realtek rtl8812au driver version 5.7.0

Supports 8811AU/8812AU and 8821AU chipsets.
Realtek RTL8814AU is not supported in this anymore!

[![Monitor mode](https://img.shields.io/badge/monitor%20mode-working-brightgreen.svg)](#)
[![Frame Injection](https://img.shields.io/badge/frame%20injection-working-brightgreen.svg)](#)
[![GitHub version](https://raster.shields.io/badge/version-v5.7.0-lightgrey.svg)](#)
[![GitHub issues](https://img.shields.io/github/issues/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/issues)
[![GitHub forks](https://img.shields.io/github/forks/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/network)
[![GitHub stars](https://img.shields.io/github/stars/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/stargazers)
[![Build Status](https://travis-ci.org/aircrack-ng/rtl8812au.svg?branch=v5.7.0)](https://travis-ci.org/aircrack-ng/rtl8812au)
[![GitHub license](https://img.shields.io/github/license/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/blob/master/LICENSE)
<br>
[![Kali](https://img.shields.io/badge/Kali-supported-blue.svg)](https://www.kali.org)
[![Arch](https://img.shields.io/badge/Arch-supported-blue.svg)](https://www.archlinux.org)
[![Armbian](https://img.shields.io/badge/Armbian-supported-blue.svg)](https://www.armbian.com)
[![ArchLinux](https://img.shields.io/badge/ArchLinux-supported-blue.svg)](https://img.shields.io/badge/ArchLinux-supported-blue.svg)
[![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](https://github.com/aircrack-ng/aircrack-ng)
[![wifite2](https://img.shields.io/badge/wifite2-supported-blue.svg)](https://github.com/kimocoder/wifite2)

Builds clean with no errors on kernels 5.4, 5.8, 5.9, 5.10.
Fully tested on kernel 5.4 with no dmesg badness, wavemon working fine, good speeds etc.
Partially tested on kernels 5.8, 5.9, 5.10 with no dmesg badness, and wavemon working fine.

### Notes
Download
```
$ git clone -b v5.7.0 https://github.com/aircrack-ng/rtl8812au.git
cd rtl*
```
Package / Build dependencies (Kali)
```
$ sudo apt-get update
$ sudo apt-get install build-essential libelf-dev linux-headers-`uname -r`
```
#### For Raspberry (RPI)

```
$ sudo apt-get install raspberrypi-kernel-headers
```

Then run this step to change platform in Makefile, For RPI 1/2/3/ & 0/Zero:
```
$ sed -i 's/CONFIG_PLATFORM_I386_PC = y/CONFIG_PLATFORM_I386_PC = n/g' Makefile
$ sed -i 's/CONFIG_PLATFORM_ARM_RPI = n/CONFIG_PLATFORM_ARM_RPI = y/g' Makefile
```

But for RPI 3B+ & 4B you will need to run those below which builds the ARM64 arch driver:
```
$ sed -i 's/CONFIG_PLATFORM_I386_PC = y/CONFIG_PLATFORM_I386_PC = n/g' Makefile
$ sed -i 's/CONFIG_PLATFORM_ARM64_RPI = n/CONFIG_PLATFORM_ARM64_RPI = y/g' Makefile
```

For setting monitor mode
  1. Fix problematic interference in monitor mode.
  ```
  $ airmon-ng check kill
  ```
  You may also uncheck the box "Automatically connect to this network when it is avaiable" in nm-connection-editor. This only works if you have a saved wifi connection.

  2. Set interface down
  ```
  $ sudo ip link set wlan0 down
  ```
  3. Set monitor mode
  ```
  $ sudo iw dev wlan0 set type monitor
  ```
  4. Set interface up
  ```
  $ sudo ip link set wlan0 up
  ```
For setting TX power
```
$ sudo iw wlan0 set txpower fixed 3000
```

### LED control

#### statically by module parameter in /etc/modprobe.d/8812au.conf or wherever, for example:

```sh
options 88XXau rtw_led_ctrl=0
```
value can be 0 or 1

#### or dynamically by writing to /proc/net/rtl8812au/$(your interface name)/led_ctrl, for example:

```sh
$ echo "0" > /proc/net/rtl8812au/$(your interface name)/led_ctrl
```
value can be 0 or 1

#### check current value:

```sh
$ cat /proc/net/rtl8812au/$(your interface name)/led_ctrl
```

### USB Mode Switch

0: doesn't switch, 1: switch from usb2.0 to usb 3.0 2: switch from usb3.0 to usb 2.0
```sh
$ rmmod 88XXau
$ modprobe 88XXau rtw_switch_usb_mode=int (0: no switch 1: switch from usb2 to usb3 2: switch from usb3 to usb2)
```

### NetworkManager

As others have noted, people using NetworkManager need to add this stanza to /etc/NetworkManager/NetworkManager.conf

```sh
[device]
wifi.scan-rand-mac-address=no
```
at the end of file /etc/NetworkManager/NetworkManager.conf and restart NetworkManager with the command:
```
$ sudo service NetworkManager restart
```

### Credits / Contributors

```
Realtek       - https://www.realtek.com.tw
Alfa Networks - https://www.alfa.com.tw
aircrack-ng   - https://www.aircrack-ng.org

astsam        - https://github.com/astsam
evilphish     - https://github.com/evilphish
fariouche     - https://github.com/fariouche
CGarces       - https://github.com/CGarces
ZerBea        - https://github.com/ZerBea
lwfinger      - https://github.com/lwfinger
Ulli-Kroll.   - https://github.com/Ulli-Kroll

```
