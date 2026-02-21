---
title: Overview
---

## AGL layers

The [AGL Project](https://www.automotivelinux.org/) is an automotive-specific
development environment that provides a Linux distribution
([AGL UCB](https://www.automotivelinux.org/software/unified-code-base)).

AGL uses layers designed to be compatible with the
[Yocto Project](https://www.yoctoproject.org) and the
[OpenEmbedded Project (OE)](https://www.openembedded.org/wiki/Main_Page).

This section provides information about the layers used by the AGL Project:

* [**`meta-agl`**](./02_meta_agl.md) : Minimal set of software needed to create an AGL distribution
  used to boot a system.
  AGL profiles are built on top of this minimal set of software.

* [**`meta-agl-demo`**](./03_meta_agl_demo.md): Provides a reference or demo platform and applications
  for the AGL Distribution.
  The reference UI is part of the [`meta-agl-demo`](./03_meta_agl_demo.md) layer.
  This layer includes AGL platform images recipe for Demo IVI.  The images recipe
  for innovative platform includes in [`meta-agl-devel`](./04_meta_agl_devel.md). (Ex. Unified HMI)

* [**`meta-agl-devel`**](./04_meta_agl_devel.md): Contains components under development or being tested.


## AGL integrated Layers

### Under external directory

```
.
├── meta-clang
├── meta-codechecker
├── meta-flutter
├── meta-lts-mixins_rust
├── meta-openembedded
├── meta-python-ai
├── meta-qt6
├── meta-ros
├── meta-security
├── meta-selinux
├── meta-spdxscanner
├── meta-tensorflow
├── meta-virtualization
├── poky
└── workspace-automation
```

* `meta-clang`: Provides support for the Clang/LLVM compiler toolchain.
* `meta-codechecker`: Provides code analysis and code quality checking tools (such as Clang Static Analyzer).
* `meta-flutter`: Provides support for Google Flutter framework.
* `meta-lts-mixins_rust`: Provides support for Rust programming language and its toolchain.
* `meta-openembedded`: Provides a comprehensive software layer from the OpenEmbedded community.
* `meta-python-ai`: Provides support for Python AI frameworks (TensorFlow, PyTorch, scikit-learn, etc.).
* `meta-qt6`: Provides support for the Qt6 framework.
* `meta-ros`: Provides support for Robot Operating System (ROS).
* `meta-security`: Provides security-related tools and utilities.
* `meta-selinux`: Provides support for SELinux (Security-Enhanced Linux) security module.
* `meta-spdxscanner`: Provides tools for license scanning and generation in SPDX (Software Package Data Exchange) format.
* `meta-tensorflow`: Provides support for TensorFlow deep learning framework.
* `meta-virtualization`: Provides support for virtualization technologies (KVM, Xen, Docker, QEMU).
* `poky`: Core layer of the Yocto Project.
* `workspace-automation`: Flutter workspace setup tooling for setting up a matching SDK environment including VScode integration.

### Under bsp directory

```
.
├── meta-agl-refhw
├── meta-arm
├── meta-aws
├── meta-freescale
├── meta-freescale-3rdparty
├── meta-lts-mixins_u-boot
├── meta-raspberrypi
├── meta-rcar
├── meta-renesas
├── meta-riscv
├── meta-rockchip
├── meta-rockchip-extra
├── meta-sifive
├── meta-sparrow-hawk
├── meta-tegra
└── meta-ti
```

* `meta-agl-refhw`: Provides BSP (Board Support Package) for AGL reference hardware.
* `meta-arm`: Provides support for ARM-based processors and SoCs.
* `meta-aws`: Provides support for Amazon Web Services (AWS) integration and services.
* `meta-freescale`: Provides BSP support for NXP Freescale/i.MX SoCs.
* `meta-freescale-3rdparty`: Provides third-party components and extensions for Freescale/i.MX boards.
* `meta-lts-mixins_u-boot`: Provides U-Boot bootloader support and customizations.
* `meta-raspberrypi`: Provides BSP support for Raspberry Pi boards.
* `meta-rcar`: Provides extra BSP support for Renesas R-Car SoCs.
* `meta-renesas`: Provides BSP support for various Renesas R-Car SoCs.
* `meta-riscv`: Provides support for various RISC-V based SoCs.
* `meta-rockchip`: Provides BSP support for Rockchip SoCs.
* `meta-rockchip-extra`: Provides additional components and extensions for NanoPC-T6 board.
* `meta-sifive`: Provides BSP support for SiFive RISC-V processors and development boards.
* `meta-sparrow-hawk`: Provides BSP support for the Sparrow Hawk automotive platform.
* `meta-tegra`: Provides BSP support for NVIDIA Tegra SoCs.
* `meta-ti`: Provides BSP support for Texas Instruments (TI) SoCs.
