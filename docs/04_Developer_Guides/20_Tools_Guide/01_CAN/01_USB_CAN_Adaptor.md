---
title: Tool guide for CAN
---

# How to send/receive CAN message using PC

## Overview

This document describe how to send CAN message to target device/view CAN message receive from target device.

We use DSD Tech [SH-C30A](https://www.deshide.com/product-details_SH-C30A.html) and [SH-C31A](https://www.deshide.com/product-details_SH-C31A.html) for reference USB-CAN adaptor.  Both adapter usage is same, this document mainly focus to SH-C30A.

## Hardware

### SH-C30A

<img src="https://wds-service-1258344699.file.myqcloud.com/20/12636/jpg/1671089335161995e3a8f14f70aad.jpg?version=0" width="400">

Connect CANH connector to target CANH connector.  Connect CANL connector to target CANL connector. Typically, GND connector does not need to target.

<img src="https://wds-service-1258344699.file.myqcloud.com/20/12636/jpg/16710893351612ad6b34acb79b956.jpg?version=1671089336" width="400">

R120 switch and Boot Switch set to these.

- Boot Switch : OFF(Work Mode)
- R120 : ON(If you do not connect terminator.)

### SH-C31A

<img src="https://wds-service-1258344699.file.myqcloud.com/20/12636/jpg/1692148935508a9d5ac2665b6a0e1cedbea2ae3cb0185.jpg?version=1692148939" width="400">


Connect CANH connector to target CANH connector.  Connect CANL connector to target CANL connector. Typically, GND connector does not need to target.

<img src="https://wds-service-1258344699.file.myqcloud.com/20/12636/jpg/16921489355978e206eeb42ed68e5870e8f03288506c8.jpg?version=1692148939" width="400">

R120 switch and Boot Switch set to these.

- Boot Switch : OFF(Work Mode)
- R120 : ON(If you do not connect terminator.)

## Software

### Install Device Driver

[SH-C30A Web page](https://www.deshide.com/product-details_SH-C30A.html)

[SH-C31A Web page](https://www.deshide.com/product-details_SH-C31A.html)

The device driver download link is described in this document.  Let's download and install driver to your windows PC.

![sw-driver](images/sw-driver.png)
![sw-driver2](images/sw-driver2.png)

As a result, this device can see at device manager. When your device has SLCAN firmware, this device can see at device manager another name. 

![sw-driver3](images/sw-driver3.png)

---
### Install Control Software

A Cangaroo is a control software for this device.  You can download this link at device page of DSD Tech.

![sw-cangaroo](images/sw-cangaroo.png)

After the install and exec, it show follow window.

![sw-cangaroo2](images/sw-cangaroo2.png)

---
### How to view CAN message receive from target device.

You do follows step, it will receive CAN message from target.

![receive1](images/receive1.png)

Select "Start Measurement".

![receive2](images/receive2.png)

Push OK.

![receive3](images/receive3.png)

It show received CAN messages.

---
### How to send CAN message to target device

You do follows step, it will send CAN message to target.

![send1](images/send1.png)

Set CAN message at Transmit View.

![send2](images/send2.png)

Push Send button, it send CAN message once.

![send3](images/send3.png)

If you want to send cyclic, you push Send Repeat after interval setting.
