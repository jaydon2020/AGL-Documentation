---
title: Building for VisionFive2 boards
---

AGL supported StarFive [VisionFive2](https://www.starfivetech.com/en/site/boards) board.  
The processor of this board is RISC-V RV64GC.

This section describes the steps you need to take to build the AGL demo image for the VisionFive2 board.

## 1. Making Sure Your Build Environment is Correct

The
"[Initializing Your Build Environment](../04_Initializing_Your_Build_Environment.md)"
section presented generic information for setting up your build environment
using the `aglsetup.sh` script.
If you are building the AGL demo image for a VisionFive2 board, you need to specify some
specific options when you run the script :

**Basic IVI demo :**

  ```sh
  $ source meta-agl/scripts/aglsetup.sh -f -m visionfive2 -b build-visionfive2 agl-demo
  $ echo "# reuse download directories" >> $AGL_TOP/site.conf
  $ echo "DL_DIR = \"$HOME/downloads/\"" >> $AGL_TOP/site.conf
  $ echo "SSTATE_DIR = \"$AGL_TOP/sstate-cache/\"" >> $AGL_TOP/site.conf
  $ ln -sf $AGL_TOP/site.conf conf/
  ```

In each case, the "-m" option specifies the machine and the list of AGL features used with script are appropriate for development of
the AGL demo image suited for VisionFive2.

## 2. Configuring the Build

Before launching the build, it is good to be sure your build
configuration is set up correctly (`./build/conf/local.conf` file).
The "[Customizing Your Build](../05_Customizing_Your_Build.md)"
section highlights some common configurations that are useful when
building any AGL image.

## 3. Using BitBake

This section shows the `bitbake` command used to build the AGL image.

Start the build using the `bitbake` command.

**NOTE:** An initial build can take many hours depending on your
CPU and and Internet connection speeds.
The build also takes approximately 200G-bytes of free disk space.

**Flutter Based IVI demo :**
The target is `agl-ivi-demo-flutter`.

```sh
$ time bitbake agl-ivi-demo-flutter
```

By default, the build process puts the resulting image in the Build Directory and further exporting that as `$IMAGE_NAME`.
Here is example for the VisionFive2 board for Flutter Based demo:

```sh
<build_dir>/tmp/deploy/images/visionfive2/agl-ivi-demo-flutter-visionfive2.rootfs.wic.xz

$ export IMAGE_NAME=aagl-ivi-demo-flutter-visionfive2.rootfs.wic.xz
```

**Qt Based IVI demo :**
The target is `agl-ivi-demo-qt`.

```sh
$ time bitbake agl-ivi-demo-qt
```

By default, the build process puts the resulting image in the Build Directory and further exporting that as `$IMAGE_NAME`.
Here is example for the VisionFive2 board for Qt Based demo:

```sh
<build_dir>/tmp/deploy/images/visionfive2/agl-ivi-demo-qt-visionfive2.rootfs.wic.xz

$ export IMAGE_NAME=agl-ivi-demo-qt-visionfive2.rootfs.wic.xz
```

## 4. Deploying the AGL Demo Image

Deploying the AGL demo image consists of copying the image on a MicroSD card,
plugging the card into the VisionFive2 board, and then booting the board.

Follow these steps to copy the image to a MicroSD card and boot
the image on the VisionFive2 board:

  1. Plug your MicroSD card into your Build Host (i.e. the system that has your build output).

  2. Extract the image into the SD card of VisionFive2 :

    **NOTE:** For VisionFive2, the image is at `<build-dir>/tmp/deploy/images/visionfive2/${IMAGE_NAME}`.

      Be sure you are root, provide the actual device name for *sdcard_device_name*, and the actual image name for *image_name*.

      ```sh
      $ lsblk
      $ sudo umount <sdcard_device_name>
      $ xzcat ${IMAGE_NAME} | sudo dd of=<sdcard_device_name> bs=4M
      $ sync
      ```

    **IMPORTANT NOTE:** Before re-writing any device on your Build Host, you need to
        be sure you are actually writing to the removable MicroSD card and not some other
        device.
        Each computer is different and removable devices can change from time to time.
        Consequently, you should repeat the previous operation with the MicroSD card to
        confirm the device name every time you write to the card.

    To summarize this example so far, we have the following:
        The first SATA drive is `/dev/sda` and `/dev/sdc` corresponds to the MicroSD card, and is also marked as a removable device.You can see this in the output of the `lsblk` command where "1" appears in the "RM" column for that device.


## Appendix.

### Serial Debugging

Initially, please refer to [VisionFive2 gpio pin assign page](https://doc-en.rvspace.org/VisionFive2/Datasheet/VisionFive_2/gpio_pin_assig.html).

Connect the cable's GND to pin 6 of the UART.  
Connect the cable's UART side RX line to pin 8.  
Connect the cable's UART side TX line to pin 10.  

The UART speed of VisionFive2 is 115200bps.

