---
title: Building the AGL Image
---

Building the AGL image involves running BitBake with a specified target.
Depending on whether you are building the image for the first time or if this
is a subsequent build, the time needed for the build could be significant.

It is critical that you specify the correct options and configurations for the
build before executing the `bitbake` command.
The previous sections in the "Image Development Workflow" have treated this setup
in a generic fashion. AGL has both `Qt` based and `HTML5` based IVI demos, where in the build process is almost the same except few changes in the build enviroment.

This section, provides links to topics with instructions needed to create images for
three types of supported platforms and for emulation/virtualization using Quick
EMUlator (QEMU) or VirtualBox:

* [x86 (Emulation and Hardware)](./02_Building_for_x86_(Emulation_and_Hardware).md)
* [Raspberry Pi 4](./03_Building_for_Raspberry_Pi_4.md)
* [R Car Gen 3](./04_Building_for_Supported_Renesas_Boards.md)
* [Virtio](./06_Building_for_Virtio.md)
* [AWS EC2 (arm64 or x86-64)](./07_Building_for_EC2_arm64_and_x86-64.md)
