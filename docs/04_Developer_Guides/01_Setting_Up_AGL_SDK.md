---
title: Setting Up AGL SDK
---

AGL provides a pre-built ready-made Software Development Kit (SDK) to help
quickstart the service and application development process.

1. Download the prebuilt SDK :

    Please open the link below and download .sh file for desired machine.

    **Note:** The links provided are for the master branch. If you want SDK for specific branch than change the name from master to specific branch.

      - **x86** : [qemux86-64](https://download.automotivelinux.org/AGL/snapshots/master/latest/qemux86-64/deploy/sdk/)

        **Note:** .sh file will be with name 	"poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-corei7-64-qemux86-64-toolchain-$(version number).sh" where version number is regularly updated on the site.


      - **ARM 32 bit** : [qemuarm](https://download.automotivelinux.org/AGL/snapshots/master/latest/qemuarm/deploy/sdk/)

        **Note:** .sh file will be with name 	"	poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-armv7vet2hf-neon-vfpv4-qemuarm-toolchain-$(version number).sh" where version number is regularly updated on the site.


      - **AARCH64 - ARM 64bit** : [qemuarm64](https://download.automotivelinux.org/AGL/snapshots/master/latest/qemuarm64/deploy/sdk/)

        **Note:** .sh file will be with name 	"	poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-aarch64-qemuarm64-toolchain-$(version number).sh" where version number is regularly updated on the site.


        *Henceforth,* **qemux86-64** *is used in these guides, unless specified
        otherwise. We also use the 'agl-demo-platform-crosssdk' as example.*

2. Create application development directory and copy SDK into them :

    **Note:** In the copy command below change the file name with name of your downloaded .sh file. In the example below file name is based on x86

    ```sh
    $ mkdir ~/agl-app
    $ cp ~/Downloads/poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-*.sh ~/agl-app/
    $ cd ~/agl-app
    ```

3. Install the downloaded SDK :

    **Note:** In commands below again change the file name based on your downloaded .sh file


    ```sh
    $ chmod 777 poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-*.sh
    $ mkdir agl-sdk/
    $ ./poky-agl-glibc-x86_64-agl-demo-platform-crosssdk-*.sh
    ```
    Select target directory for SDK : `~/agl-app/agl-sdk`

    ```sh
    Automotive Grade Linux SDK installer version 14.0.0
    =============================================================
    Enter target directory for SDK (default: /opt/agl-sdk/10.90.0+snapshot-corei7-64): ~/agl-app/agl-sdk
    You are about to install the SDK to "/home/boron/agl-app/agl-sdk". Proceed [Y/n]? Y
    Extracting SDK..........................................................................................................................................done
    Setting it up...done
    SDK has been successfully set up and is ready to be used.
    Each time you wish to use the SDK in a new shell session, you need to source the environment setup script e.g.
    $ . /home/boron/agl-app/agl-sdk/environment-setup-corei7-64-agl-linux
    ```

4. Source the SDK environment setup, each time you wish to use the SDK in a new shell session :

    ```sh
    $ source ~/agl-app/agl-sdk/environment-setup-corei7-64-agl-linux
    ```
