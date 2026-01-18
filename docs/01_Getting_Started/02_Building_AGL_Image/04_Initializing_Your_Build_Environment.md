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

Following are brief descriptions of the AGL features you can specify on the
`aglsetup.sh` command line:

* **agl-all-features**: A set of AGL default features.
  Do not think of this set of features as all the AGL features.

* **agl-app-framework**: Application Framework

* **agl-archiver**: Enables the archiver class for releases.

* **agl-ci**: Flags used for Continuous Integration (CI).
  Using this feature changes the value of the
  [`IMAGE_FSTYPES`](https://yoctoproject.org/docs/3.1.4/ref-manual/ref-manual.html#var-IMAGE_FSTYPES)
  variable.

* **agl-ci-change-features**: Enables features for CI builds for Gerrit changes.

* **agl-ci-change-features-nogfx**: Enables features for CI builds for Gerrit changes
  for targets that use binary graphics drivers (i.e. builds without graphics).

* **agl-ci-snapshot-features**: Enables features for CI daily snapshot builds.

* **agl-ci-snapshot-features-nogfx**: Enables features for CI daily snapshot builds for
  targets that use binary graphics drivers (i.e. builds without graphics).

* **agl-devel**: Activates development options such as an empty root password,
  debuggers, strace, valgrind, and so forth.

* **agl-netboot**: Enables network boot support through Trivial File Transfer Protocol (TFTP) and Network Block Device (NBD) protocol.
  Netboot is needed for CI and useful for development to avoid writing
  sdcards. Needs additional setup.

* **agl-ptest**: Enables
  [Ptest](https://docs.yoctoproject.org/dev-manual/common-tasks.html#testing-packages-with-ptest)
  as part of the build.

* **agl-demo**: Enables the layers meta-agl-demo and meta-qt5.
  You need agl-demo if you are going to build the agl-ivi-demo-qt.

* **agl-pipewire**: Enables AGLs pipewire support.

* **agl-localdev**: Adds a local layer named "meta-localdev" in the
  meta directory and a local.dev.inc configuration file when that file
  is present.

  This feature provides a shortcut for using the layer meta-localdev
  in the top-level folder for easy modifications to your own recipes.

## Example

Following is an example that initializes the build environment, selects "beaglebone"
for the machine, and chooses the "agl-demo" feature, which also includes the
"agl-appfw-smack", "agl-devel", and "agl-hmi-framework" features:

```sh
$ source meta-agl/scripts/aglsetup.sh -m qemux86-64 -b qemux86-64 agl-demo agl-devel
aglsetup.sh: Starting
Generating configuration files:
   Build dir: /home/scottrif/workspace_agl/build
   Machine: qemux86-64
   Features: agl-appfw-smack agl-demo agl-devel
   Running /home/scottrif/workspace_agl/poky/oe-init-build-env
   Templates dir: /home/scottrif/workspace_agl/meta-agl/templates/base
   Config: /home/scottrif/workspace_agl/build/conf/bblayers.conf
   Config: /home/scottrif/workspace_agl/build/conf/local.conf
   Setup script: /home/scottrif/workspace_agl/build/conf/setup.sh
   Executing setup script ... --- beginning of setup script
 fragment /home/scottrif/workspace_agl/meta-agl/templates/base/01_setup_EULAfunc.sh
 fragment /home/scottrif/workspace_agl/meta-agl/templates/base/99_setup_EULAconf.sh
 end of setup script
OK
Generating setup file: /home/scottrif/workspace_agl/build/agl-init-build-env ... OK
aglsetup.sh: Done
 Shell environment set up for builds.
You can now run 'bitbake target'
Common targets are:
- meta-agl layer:
  - included by default
    * agl-image-boot                (just enough to boot)
    * agl-image-minimal             (minimal filesystem with APIs)
    * agl-image-minimal-crosssdk    (crosssdk for ^^)

    * agl-image-weston              (minimal filesystem with weston)
    * agl-image-compositor          (minimal filesystem with AGL compositor)

- meta-agl-demo:                    (IVI demo with UI)
  - with 'agl-demo'
    * agl-ivi-image                 (base for IVI targets)
    * agl-ivi-image-crosssdk        (sdk for ^^)

    * agl-ivi-demo-qt               (IVI Qt demo image)
    * agl-ivi-demo-qt-crosssdk      (sdk for ^^)
    * agl-ivi-demo-flutter          (IVI Flutter demo image)
    * agl-ivi-demo-html5            (IVI HTML5 demo image)

    * agl-cluster-demo-qt           (cluster Qt demo image)
    * agl-cluster-demo-flutter      (cluster Flutter demo image)

    * agl-telematics-demo           (telematics demo image)

    * agl-gateway-demo              (gateway demo image)
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
