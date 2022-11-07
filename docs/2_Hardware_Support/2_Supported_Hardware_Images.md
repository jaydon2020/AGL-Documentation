---
title: Supported Hardware Images
---



### Supported Hardware Images

AGL supports a variety of interfaces, each requiring unique setup configuration.

#### 1. In-Vehicle Infotainment (IVI)

**Supported boards** :

AGL Reference Boards [QEMU, RCar Gen 3, Raspberry Pi 4](./Overview.md), & agl-refhw

Community supported Boards [BBE, i. MX 6, i. MX 8](./Overview.md)

* Qt Based :

    * Setting up flags at `aglsetup` script :

        ```sh
        $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo

        #To enable Developer Options
        $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo agl-devel
        ```

    * Building target image :

        ```sh
        $ time bitbake agl-demo-platform
        ```

* HTML5 Based :

    * Setting up flags at `aglsetup` script :

        ```sh
        $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo

        # To enable Developer Options
        $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo agl-devel
        ```

    * Building target image :

        ```sh
        $ time bitbake agl-demo-platform-html5
        ```


#### 2. Instrument Cluster

**Supported boards** :

AGL Reference Boards [QEMU, RCar Gen 3, & Raspberry Pi 4](./Overview.md)

* Setting up flags at `aglsetup` script :

    ```sh
    $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo

    # To enable Developer Options
    $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo agl-devel
    ```

* Building target image :

    ```sh
    $ time bitbake agl-cluster-demo
    ```

#### 3. Telematics

Headless demo platform for low-spec boards.

**Supported boards** :

Community supported Boards [BeagleBone](./Overview.md)


* Setting up flags at `aglsetup` script :

    ```sh
    $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo

    # To enable Developer Options
    $ source meta-agl/scripts/aglsetup.sh -f -m $MACHINE -b build-$MACHINE agl-demo agl-devel
    ```

* Building target image :

    ```sh
    $ time bitbake agl-telematics-demo
    ```
