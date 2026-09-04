# openwrt-eap225-outdoor-v3

I successfully installed OpenWrt on an EAP225-Outdoor running the latest TP-Link firmware. The latest firmware blocks installation of both older TP-Link firmware and third-party firmware such as OpenWrt. This guide describes a way to deviate from the official firmware regardless.

### Required Materials

| ![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/USB-TTL1.png) | DSD TECH SH-U09C5 USB to TTL UART converter cable with FTDI chip, supports 5V 3.3V 2.5V 1.8V TTL | <https://amzn.eu/d/0cvFHawQ> |
| --- | --- | --- |
| ![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/4PinProgrammer.png) | AZDelivery 4-pin programmer I2C module test tool PCB clip, 1× 4P gold-plated pogo pins | <https://amzn.eu/d/02t9oDBm> |
| Small flat-blade screwdriver | | |
| Switch with or without PoE | | |

### Required Files

| Initramfs (for U-Boot / RAM) | [OpenWrt 23.05.5 "initramfs"](https://downloads.openwrt.org/releases/23.05.5/targets/ath79/generic/openwrt-23.05.5-ath79-generic-tplink_eap225-outdoor-v3-initramfs-kernel.bin) | NOTE: flash an older version first. The newer ones are too large. |
| --- | --- | --- |
| Sysupgrade (later, for LuCI) | [OpenWrt 25.12.1 "sysupgrade"](https://downloads.openwrt.org/releases/25.12.1/targets/ath79/generic/openwrt-25.12.1-ath79-generic-tplink_eap225-outdoor-v3-squashfs-sysupgrade.bin) | |

### Preparation

- Install picocom
- Install tftp
- Configure tftp and copy the files
- Set the USB-to-TTL stick to 3.3V
- Set up the network connection and configure the IPs 192.168.0.66/24, 192.168.0.100/24, and 192.168.1.2/24. *The EAP pulls the firmware via TFTP over either 192.168.0.66 or 192.168.0.100. Afterward, 192.168.1.1 is OpenWrt's default IP.*

```
apt update
apt install picocom
apt install tftp-hpa
```

```
cp /FOLDER/openwrt-23.05.5-ath79-generic-tplink_eap225-outdoor-v3-initramfs-kernel.bin /srv/tftp/initramfs.bin
```

*Overly long filenames can cause errors. tftp may be blocked by the firewall — run a connectivity test before copying.*

### Opening the EAP225-Outdoor

There are sealing stickers under the antennas. Remove them. Underneath the stickers are slotted nuts. Insert the screwdriver into the slot and unscrew the nut by turning it. Then remove the washer and pull the EAP225-Outdoor housing off downward. *Caution: secure the 2 outer and 2 inner sealing rings as you slide the EAP225 off downward.*

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20zerlegen.jpeg)

### EAP225 Board

There are 4 pins on the board: TX, RX, GND, and VDD. **WARNING: NEVER CONNECT THE VDD PIN**

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20platine.jpeg)

### Connecting the Pogo Pins

Place the clip with the pins onto the board. Swap the TX and RX pins between the board and the USB stick.
TX -> RX
RX -> TX
GND -> GND

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20anschluss.jpeg) ![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/USB-TTL2.jpeg)

### Starting Picocom

`sudo picocom -b 115200 --flow n --parity n --databits 8 /dev/ttyUSB0`

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/picocom%20rdy.png)

### Powering the EAP225-Outdoor

Power the EAP225-Outdoor via PoE and watch the console output. It should look like this:

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20first%20boot.png)

If the output matches, disconnect power, and before reconnecting, press and hold **CTRL+B**. Then power the EAP225 back on. You'll then land in the EAP225's console.

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20console.png)

### Loading the Firmware from the TFTP Server (Local Machine)

`tftp 0x82000000 initramfs.bin`
*Occasionally the console reports that it doesn't recognize the tftp command — trying again usually works. In the example image the filename is slightly altered.*

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20copy%20firmware.png)

### Loading the Firmware into Memory

`bootelf`

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/EAP225-outdoor%20install%20firmware.png)

### Starting OpenWrt

After 1–2 minutes, OpenWrt should be reachable.

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/LuCi%20Login.png)

Once it's running, the firmware still needs to be fully installed — so far only the initramfs is loaded. After a power loss, the device forgets this.

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/LuCi%20Flash%20Firmware1.png)

![](https://github.com/Kollos2k/openwrt-eap225-outdoor-v3/raw/main/images/LuCi%20Flash%20Firmware2.png)

Download the latest firmware and install it via sysupgrade.
