---
layout: post
title: "lantern MacOS/Linux"
author: iosdevlog
date: 2017-12-04 16:50:05 +0800
description: ""
category: lantern
tags: [MacOS, Linux]
---

![lantern](/assets/software/lantern/lantern.png)

We can change Mac Address to reset lantern's free 500M network flow.

# SpoofMac <https://github.com/feross/SpoofMAC>

```
$ [sudo] pip install SpoofMAC
```

# Examples

Some short usage examples.

## List available devices:

```
spoof-mac.py list
- "Ethernet" on device "en0" with MAC address A8:60:B6:03:31:68
- "Wi-Fi" on device "en1" with MAC address 28:F0:76:5E:D2:F0
- "Bluetooth PAN" on device "en4" with MAC address 28:F0:76:5E:D2:F1
- "Thunderbolt 1" on device "en2" with MAC address 1A:00:02:51:BE:00
- "Thunderbolt 2" on device "en3" with MAC address 1A:00:02:51:BE:01
- "Thunderbolt Bridge" on device "bridge0" with MAC address 1A:00:02:51:BE:00
```

## Randomize MAC address (requires root)

```
$ sudo spoof-mac.py randomize en0
$ spoof-mac.py list
- "Ethernet" on device "en0" with MAC address A8:60:B6:03:31:68 currently set to 00:0F:4B:7A:37:9A
- "Wi-Fi" on device "en1" with MAC address 28:F0:76:5E:D2:F0
- "Bluetooth PAN" on device "en4" with MAC address 28:F0:76:5E:D2:F1
- "Thunderbolt 1" on device "en2" with MAC address 1A:00:02:51:BE:00
- "Thunderbolt 2" on device "en3" with MAC address 1A:00:02:51:BE:01
- "Thunderbolt Bridge" on device "bridge0" with MAC address 1A:00:02:51:BE:00
```
