---
title: Initializing Your Build Environment
---

Part of the downloaded AGL software is a setup script that you must
run to initialize the build environment.

## `aglsetup.sh` Script

You can find this script here:

```sh
$AGL_TOP/master/meta-agl/scripts/aglsetup.sh
```

The script accepts many options that allow you to define build parameters such
as the target hardware (i.e. the machine), build directory, and so forth.
Use the following commands to see the available options and script syntax:

```sh
$ cd $AGL_TOP/master
$ source meta-agl/scripts/aglsetup.sh -h
```

## AGL Machines (board support)

Your target platform will be selected with the `-m` flag.
The MACHINE can be selected from the templates in `meta-agl/templates/machine/*`.
Note: This is also the place where you can add new boards.

Following is a list of the available machines (level of support varies!):

```sh
Available machines:
   [meta-agl]
       am62xx-evm
       aws-ec2-arm64
       aws-ec2-x86-64
       beaglebone-ai64
       beagleplay
       ebisu                      # Renesas RCar Ebisu
       generic-arm64
       h3-salvator-x              # Renesas RCar Salvator/H3
       h3ulcb                     # Renesas RCar H3
       h3ulcb-kf                  # Renesas RCar H3 w Kingfisher Board
       h3ulcb-nogfx               # Renesas RCar H3 w/o gfx blobs
       imx8mqevk                  # i.MX8 w etnaviv
       intel-corei7-64            # x86-64 (Intel flavour)
       j721e-evm
       jetson-agx-orin-devkit
       m3-salvator-x              # Renesas RCar Salvator/M3
       m3ulcb                     # Renesas RCar M3
       m3ulcb-kf                  # Renesas RCar M3 w Kingfisher Board
       m3ulcb-nogfx               # Renesas RCar M3 w/o gfx blobs
       nanopc-t6                  # Friendly Elec NanoPC-T6 (Rockchip RK3588)
       qemuarm                    # Qemu ARM
       qemuarm64                  # Qemu AArch 64 (ARM 64bit)
       qemuriscv64                # Qemu RISC-V 64
       qemux86-64                 # Qemu x86-64
       raspberrypi4               # Raspberry Pi 4
       raspberrypi5               # Raspberry Pi 5
       raspberrypi500             # Raspberry Pi 500
       s4sk
       sparrow-hawk               # Retronix Sparrow Hawk (Renesas RCar V4H)
       spider
       unmatched                  # RISC-V unmatched
       virtio-aarch64             # ARM64 Virtio Guest
       visionfive2                # RISC-V Vision Five 2

```

## AGL Features

Before running the `aglsetup.sh`, you should understand what AGL features you
want to include as part of your image.
The script's help output lists available features and shows you the layers in
which they reside.

Following is a list of the available features:

```sh
Available features:
   [meta-agl]                                    # CORE layer
       Refer: https://git.automotivelinux.org/AGL/meta-agl/tree/templates/feature
       agl-all-features                          # For the usual demo image
       agl-app-framework                         # Application Framework
       agl-archiver                              # Source Archiver
       agl-buildstats                            # Build Statistics
       agl-ci-change-features-nogfx              # CI: features enabled for gerrit change builds w/o  gfx
       agl-ci-change-features                    # CI: features enabled for gerrit change builds with gfx
       agl-ci-snapshot-features-nogfx            # CI: features enabled for nightly builds w/o  gfx
       agl-ci-snapshot-features                  # CI: features enabled for nightly builds with gfx
       agl-ci                                    # CI: specific settings for the CI environment
       agl-create-spdx                           # Software Bill of Materials (SBOM) generation
       agl-devel :( agl-package-management )     # Developer Env (root login w/o pass)
       agl-drm-lease                             # Display (DRM master) sharing
       agl-flutter                               # Flutter support
       agl-fossdriver                            # Fossology integration, Scancode, etc.
       agl-ic                                    # Core feature for instrument cluster
       agl-kuksa-val                             # Kuksa-val
       agl-localdev                              # inclusion of local development folder ./meta-localdev
       agl-netboot                               # network boot (e.g. in CI)
       agl-package-management                    # include package management (e.g. rpm)
       agl-pipewire                              # include pipewire
       agl-prhash-public
       agl-ptest                                 # enable ptest pckages
       agl-refhw-h3                              # enable reference hardware when building h3ulcb machine
       agl-selinux                               # Enables building with SELinux
       agl-virt                                  # EG-Virt features

   [meta-agl-demo]                               # DEMO layer
       Refer: https://git.automotivelinux.org/AGL/meta-agl-demo/tree/templates/feature
       agl-container-guest-demo                  # Enable container guest support in eache IVI demo images
       agl-demo                                  # default IVI demo 
       agl-kvm                                   # Enables support for building multiconfig based KVM+QEMU demo images

   [meta-agl-devel]                              # Development layer
       Refer: https://git.automotivelinux.org/AGL/meta-agl-devel/tree/templates/feature
       agl-cef
       agl-egvirt                                # EG-Virt feature
       agl-ic-container                          # AGL Instrument Cluster Container Integration feature
       agl-offline-voice-agent                   # Feature template for meta-offline-voice-agent layer
       agl-rdp
       agl-ros2
       agl-test                                  # Test framework under development
       agl-uhmi                                  # Unified HMI
       agl-uhmi-demo                             # Unified HMI demo

```

To find out exactly what a feature provides, check out the respective layer and its README.

An AGL feature is a configuration that accounts for specific settings
and dependencies needed for a particular build.
For example, specifying the "agl-demo" feature makes sure that the
`aglsetup.sh` script creates configuration files needed to build the
image for the AGL demo.

Typical combination for AGL features.

* **agl-demo**: Setup AGL demo IVI building environment.

* **agl-demo agl-devel**: Setup AGL demo IVI building environment for development.

* **agl-demo agl-ic**: Setup AGL demo IVI building environment with Instrument Cluster feature.

* **agl-ic-container**: Setup AGL Instrument Cluster Container Integration building environment. Ref. to Build_and_Boot_guide_Profile.

* **agl-demo agl-container-guest-demo agl-demo**: Setup AGL demo IVI building environment for container guest. Ref. to Build_and_Boot_guide_Profile.

* **agl-kvm**: Setup AGL kvm demo building environment that integrates demo IVI and demo Instrument Cluster into one environment. Ref. to Build_and_Boot_guide_Profile.


## Example

Following is an example that initializes the build environment, selects "qemux86-64"
for the machine, and chooses the "agl-demo" feature, which also includes the "agl-pipewire"
, "agl-app-framework", "agl-selinux", "agl-kuksa-val" and "agl-flutter" features:

```sh
$ source meta-agl/scripts/aglsetup.sh -m qemux86-64 -b qemux86-64 agl-demo
------------ aglsetup.sh: Starting
Generating configuration files:
   Build dir: /agl/master/tree/qemux86-64
   Machine: qemux86-64
   Features: agl-app-framework agl-demo agl-flutter agl-kuksa-val agl-pipewire agl-selinux
   Running /agl/master/tree/external/poky/oe-init-build-env
   Templates dir: /agl/master/tree/meta-agl/meta-agl-core/conf/templates/base
   Config: /agl/master/tree/qemux86-64/conf/bblayers.conf
   Config: /agl/master/tree/qemux86-64/conf/local.conf
   Setup script: /cross/sata/agl/master/tree/qemux86-64/conf/setup.sh
   Executing setup script ... --- beginning of setup script
--- fragment /agl/master/tree/meta-agl/templates/base/01_setup_EULAfunc.sh
--- fragment /agl/master/tree/meta-agl/templates/base/01_setup_pkg_revision.sh
--- fragment /agl/master/tree/meta-agl/templates/base/99_setup_EULAconf.sh
--- end of setup script
OK
Generating setup manifest: /agl/master/tree/qemux86-64/aglsetup.manifest ... OK
Generating setup file: /agl/master/tree/qemux86-64/agl-init-build-env ... OK
------------ aglsetup.sh: Done
Common targets are:
- meta-agl layer:
  * agl-image-boot                (just enough to boot)
  * agl-image-minimal             (minimal filesystem with APIs)
  * agl-image-minimal-crosssdk    (crosssdk for above)
  * agl-image-weston              (minimal filesystem with weston)
  * agl-image-compositor          (minimal filesystem with AGL compositor)

- meta-agl-flutter:
  * agl-image-flutter.bb          (example/test minimal Flutter image)
  * agl-image-flutter-debug.bb    (above with debugging support)

- meta-agl-demo:
  * agl-ivi-image                 (base for IVI targets)
  * agl-ivi-image-crosssdk        (SDK for ^^)
  * agl-ivi-demo-qt               (IVI Qt demo image)
  * agl-ivi-demo-qt-crosssdk      (SDK for ^^)
  * agl-ivi-demo-flutter          (IVI Flutter demo image)
  * agl-cluster-demo-qt           (cluster Qt demo image)
  * agl-cluster-demo-flutter      (cluster Flutter demo image)
  * agl-ivi-demo-control-panel    (demo/test control panel image)

  More details:
  https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/07_Available_Demo_Images

- meta-agl-demo-shared:
  * agl-instrument-cluster-standalone-demo
    (IC EG non-container cluster test image)

Build guide:
https://docs.automotivelinux.org/en/master/#01_Getting_Started/02_Building_AGL_Image/01_Build_Process_Overview/

```

Running the script creates the Build Directory if it does not already exist.
The default Build Directory is `$AGL_TOP/<release-branch-name>/build`, and the nomenclature to be used throughout this doc is going to be `$AGL_TOP/<release-branch-name>/<build-dir>`
For this example, the Build Directory is `$AGL_TOP/master/qemux86-64`.

The script's output also indicates the machine and AGL features selected for the build.

The script creates two primary configuration files used for the build: `local.conf` and `bblayers.conf`.
Both these configuration files are located in the Build Directory in the `conf` folder.
If you were to examine these files, you would find standard Yocto Project
configurations along with AGL configuration fragments, which are driven by the
machine (i.e. beaglebone) and the AGL features specified as part of the
script's command line.

The end result is configuration files specific for your build in the AGL development environment.

Finally, part of the `aglsetup.sh` script makes sure that any End User License Agreements (EULA)
are considered.
You can see that processing in the script's output as well.

**NOTE:** Use of the `local.conf` and `bblayers.conf` configuration files is fundamental
in the Yocto Project build environment.
Consequently, it is fundamental in the AGL build environment.
You can find lots of information on configuring builds in the Yocto Project
documentation set.
Here are some references if you want to dig into configuration further:

* [Customizing Images Using local.conf](https://docs.yoctoproject.org/dev-manual/common-tasks.html#customizing-images-using-local-conf)
* [Local](https://docs.yoctoproject.org/ref-manual/varlocality.html#local)
* [build/conf/local.conf](https://docs.yoctoproject.org/ref-manual/structure.html#build-conf-local-conf)
* [build/conf/bblayers.conf](https://docs.yoctoproject.org/ref-manual/structure.html#build-conf-bblayers-conf)
* [BBLAYERS](https://docs.yoctoproject.org/ref-manual/variables.html#term-BBLAYERS)
* [User Configuration](https://docs.yoctoproject.org/ref-manual/structure.html#build)
* [Enabling Your Layer](https://docs.yoctoproject.org/dev-manual/common-tasks.html#enabling-your-layer)
