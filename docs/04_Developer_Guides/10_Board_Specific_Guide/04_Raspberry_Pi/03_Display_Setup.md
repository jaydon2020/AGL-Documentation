---
title: Display setup
---

## Display setup on RPi4

This assumes that you'll be using the Raspberry Pi 7'' display. Installation
can be found at [Rpi Display page](https://www.raspberrypi.com/documentation/accessories/display.html).

If booting over the network, the dtb should already contain the ft5406 dtb,
while booting over sd-card the following show be in the /boot/config.txt
file:

	dtoverlay=rpi-ft5406-overlay

Once the board boots up, you should get a rainbow like effect and afterwards
booting up would display debug scrolling over. You'll need to adjust the
orientation as the 800x480 is in portrait.  Edit /etc/xdg/weston/weston.ini and
add a new output entry, like the following:

	[output]
	name=DSI-1
	transform=90

Note that for the Qt platform, the homescreen application, together with the
other demo applications, is tailored specifically for a 1080p display and it
will display incorrectly on such a smaller display.
