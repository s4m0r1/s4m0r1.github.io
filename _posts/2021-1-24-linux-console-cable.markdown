---
layout: post
title:  "Linuxでコンソールケーブルを使う"
date:   2020-11-01 21:42:24 +0900
categories: Linux
---

## はじめに

ルーターなどのメンテナンスによく使用されるコンソールケーブルですが、Linuxで使用するタイミングが来たので試してみる

## 環境

```
cat /etc/lsb-release                                                                                                        2021年01月24日 18時28分22秒
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04.1 LTS"
```

## screenコマンドを使って接続

よく使われる方式はこれっぽいですね

使用するケーブルのデバイス名を使用して接続

だいたい`ttyUSBX`でしょみたいなのはあんまよくなさそうなので`dmesg`で最後に接続されたデバイスを見てみましょう

（ パイプでtailなどに渡すとわざわざ長いのを見せられずに済む）

```
[ 8550.813769] usb 1-1.4.2: Product: USB HS Serial Converter
[ 8550.813771] usb 1-1.4.2: Manufacturer: FTDI
[ 8550.813773] usb 1-1.4.2: SerialNumber: FTBUIT8I
[ 8551.466736] usbcore: registered new interface driver usbserial_generic
[ 8551.466759] usbserial: USB Serial support registered for generic
[ 8551.526468] usbcore: registered new interface driver ftdi_sio
[ 8551.526620] usbserial: USB Serial support registered for FTDI USB Serial Device
[ 8551.526836] ftdi_sio 1-1.4.2:1.0: FTDI USB Serial Device converter detected
[ 8551.527548] usb 1-1.4.2: Detected FT232BM
[ 8551.529269] usb 1-1.4.2: FTDI USB Serial Device converter now attached to ttyUSB0
```

一番最後の行から`ttyUSB0`みたいですね実際にscreenに渡して使ってみましょう

デバイスの権限は`root`になっていることが多いので`sudo`使うか権限を変えておきましょう

引数にデバイスとポーレートを指定して実行します

```
sudo screen /dev/ttyUSB0 9600

Starting with exec0 and config0 ...

RTX1500 Rev.8.03.90 (Fri Jun 25 10:31:59 2010)
  Copyright (c) 1994-2010 Yamaha Corporation. All Rights Reserved.
  Copyright (c) 1991-1997 Regents of the University of California.
  Copyright (c) 1995-2004 Jean-loup Gailly and Mark Adler.
  Copyright (c) 1998-2000 Tokyo Institute of Technology.
  Copyright (c) 2000 Japan Advanced Institute of Science and Technology, HOKURIKU.
  Copyright (c) 2002 RSA Security Inc. All rights reserved.
  Copyright (c) 1997-2004 University of Cambridge. All rights reserved.
  Copyright (C) 1997 - 2002, Makoto Matsumoto and Takuji Nishimura, All rights reserved.
  Copyright (c) 1995 Tatu Ylonen , Espoo, Finland All rights reserved.
  Copyright (c) 1998-2004 The OpenSSL Project.  All rights reserved.
  Copyright (C) 1995-1998 Eric Young (eay@cryptsoft.com) All rights reserved.
00:a0:de:35:9c:7f, 00:a0:de:35:9c:80, 00:a0:de:35:9c:81, 
Memory 128Mbytes, 3LAN, 2BRI

# show config 
# RTX1500 Rev.8.03.90 (Fri Jun 25 10:31:59 2010)
# MAC Address : 00:a0:de:35:9c:7f, 00:a0:de:35:9c:80, 00:a0:de:35:9c:81, 
# Memory 128Mbytes, 3LAN, 2BRI
# main:  RTX1500 ver=d0 serial=N0Y011661 MAC-Address=00:a0:de:35:9c:7f MAC-Addr
ess=00:a0:de:35:9c:80 MAC-Address=00:a0:de:35:9c:81
# Reporting Date: Jan 1 09:01:40 1980
login password encrypted *
administrator password encrypted *
console character ascii
ip route default gateway 172.16.0.1
ip lan1 address 10.17.127.1/24
ip lan2 address 172.16.0.60/24
ip lan2 nat descriptor 1
nat descriptor type 1 masquerade
nat descriptor address outer 1 172.16.0.60
nat descriptor address inner 1 10.17.127.1-10.17.127.254
nat descriptor masquerade static 1 1 10.17.127.4 tcp www
dhcp service server
dhcp scope 1 10.17.127.2-10.17.127.50/24
dns server 1.1.1.1
```

終了する場合は`Ctrl+a`おしてから`k`その後メッセージが出るので接続を切っていい場合は`y`を入力
