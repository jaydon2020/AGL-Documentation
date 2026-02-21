---
title: Overview
---

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
  This layer also contains software packages that OEMs develop for inclusion into AGL.
