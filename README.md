# openwrt-eap225-outdoor-v3

Ich habe erfolgreich openwrt auf einen eap225-outdoor mit der neusten tplink firmware installiert.....

<h3>Benötigtes Material</h3>
<table>
<tr><td><img src="./images/USB-TTL1.png" width="50px"></td><td>DSD TECH SH-U09C5 USB zu TTL UART Konverter Kabel mit FTDI Chip Unterstützung 5V 3.3V 2.5V 1.8V TTL</td><td>https://amzn.eu/d/0cvFHawQ</td></tr>
<tr><td><img src="./images/4PinProgrammer.png" width="50px"></td><td>AZDelivery 4 Pin Programmer I2C Modul Test Werkzeug PCB Klemme 1 * 4P Gold-beschichtete Pogo Pins</td><td>https://amzn.eu/d/02t9oDBm</td></tr>
</table>
<h3>Benötigte Dateien</h3>
<table>
<tr><td>Initramfs (für U-Boot / RAM)</td><td><a href="https://downloads.openwrt.org/releases/23.05.5/targets/ath79/generic/openwrt-23.05.5-ath79-generic-tplink_eap225-outdoor-v3-initramfs-kernel.bin">OpenWrt 23.05.5 "initramfs"</a></td><td>ACHTUNG erst mal eine alte version flashen. Die neuen sind zu groß.</td></tr>

<tr><td>Sysupgrade (später für LuCI)</td><td><a href="https://downloads.openwrt.org/releases/25.12.1/targets/ath79/generic/openwrt-25.12.1-ath79-generic-tplink_eap225-outdoor-v3-squashfs-sysupgrade.bin">OpenWrt 25.12.1 "sysupgrade"</a></td><td></td></tr>
</table>


<h3>Vorbereitung</h3>
<ul><li>Installieren von picocom</li><li>installieren von tftp</li><li>tftp configurieren und datien kopieren</li></ul>

<code>apt update
apt install picocom
apt install tftp-hpa
</code>
<code>
cp /FOLDER/openwrt-23.05.5-ath79-generic-tplink_eap225-outdoor-v3-initramfs-kernel.bin /srv/tftp/initrams.bin
</code>
<i>Zu lange Dateinamen können Fehler verursachen. tftp kann durch die Firewall geblockt werden. Vor kopieren Funktionstest durchführen.</i>
