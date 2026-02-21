---
title: meta-agl
---

## Introduction

The `meta-agl` layer provides the minimal set of software
to boot an AGL Distribution system.
You use this layer as the minimal core on which to build AGL profiles.

**NOTE:** The `meta-agl` layer does not include a reference UI.
  The reference UI is included as part of the
  [`meta-agl-demo`](03_meta_agl_demo.md) layer.
  Furthermore, `meta-agl` does not include additional components, such
  as security, which are part of the
  `meta-agl-extra` layer.

## Sub-Layers

The `meta-agl` layer itself contains many sub-layers and files.
Following is a "tree" look at the layer:

```
.
├── LICENSE
├── LICENSE.GPL-2.0-only
├── LICENSE.MIT
├── README-AGL.md
├── README.md -> meta-agl.md
├── agl-layers-overview.md
├── docs
├── meta-agl-bsp
├── meta-agl-core
├── meta-agl-core-test
├── meta-agl-ic
├── meta-agl-ivi
├── meta-agl.md
├── meta-netboot
├── meta-pipewire
├── scripts
├── templates
```

This list provides some overview information on the files and sub-layers
in `meta-agl`:

* LICENSE* : licenses of the various components.
* README* : RTFM
* agl-layers-overview.md: references to the various layers that make up AGL
* `docs`: Contains files that support AGL documentation.
* `meta-agl-bsp`: Contains adaptations for recipes and required packages
  to boot an AGL distribution on targeted hardware and emulation (i.e. QEMU).
* `meta-agl-core`: This is the core layer with essential recipes and the distro.
* `meta-agl-core-test`: recipes supporting qa images (separate layer due to external dependencies)
* `meta-agl-ic`: Instrument Cluster platform
* `meta-agl-ivi`: IVI platform
* `meta-netboot`: Contains recipes and configuration adjustments to allow network
  boot through network block device (NBD) since network file system (NFS) does not
  support security labels.
* `meta-pipewire`: Configuration and recipes supporting pipewire as audio manager
* `scripts`: AGL development setup and support scripts.
* `templates`: Base, feature, and machine templates used in the AGL development
  environment.

## Packagegroups

This section describes the AGL
[packagegroup](https://docs.yoctoproject.org/dev-manual/common-tasks.html#customizing-images-using-custom-package-groups)
design:

### core non-ui packagegroups
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-core-connectivity.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-core-devel.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-core-security.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-core-os-commonlibs.bb

### graphical subsystem
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-profile-graphical.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-graphical-multimedia.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-graphical-weston.bb

### image-related packagegroups
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-core-boot.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-image-boot.bb
* meta-agl-core/recipes-platform/packagegroups/packagegroup-agl-image-minimal.bb

### QA/Test related packagegroups
* meta-agl-core-test/recipes-test/packagegroups
* meta-agl-core-test/recipes-test/packagegroups/packagegroup-agl-test.bb

## Images

### (Barely) bootable image
* meta-agl-core/recipes-platform/images/agl-image-boot.bb

### minimal image and SDK (console)
* meta-agl-core/recipes-platform/images/agl-image-minimal.bb
* meta-agl-core/recipes-platform/images/agl-image-minimal-crosssdk.bb

### weston-based image for re-use
* meta-agl-core/recipes-platform/images/agl-image-weston.bb

### image for use in the YP autobuilder
* meta-agl-core/recipes-platform/images/agl-image-core-autobuilder.bb

### image with extra QA tooling (e.g. to run LTP)
* meta-agl-core-test/images/agl-image-minimal-qa.bb



**Note:** For update details on this page please refer

<https://git.automotivelinux.org/AGL/meta-agl/tree/meta-agl.md>
