---
title: Available Demo Images
---

## 1. Demo Images

These images are available when the `agl-demo` feature is given to `aglsetup.sh`.  The resulting images use DHCP for network configuration, and all except the `agl-ivi-demo-control-panel` images will contain instances of the KUKSA.val databroker by default.

### agl-ivi-image

Base IVI demo image that the other IVI images derive from.  A user will typically not be building this image, it is more relevant if making changes to the base demo services or packaging that affects all the IVI demos.

### agl-ivi-image-crosssdk

Corresponding SDK image for `agl-ivi-image`.  Typically, a user will be more interested in the `agl-ivi-demo-qt-crosssk`, as the resulting SDK from that image is more useful for application development.

### agl-ivi-image-flutter

Base Flutter IVI demo image.  Derived from `agl-ivi-image` and includes the Flutter components required for running Flutter applications.  A user will typically not be building this image, it usually will only be interesting if doing development of primary Flutter applications such as IVI homescreen  applications.

### agl-ivi-demo-flutter

Flutter based IVI demo image, using `flutter-ics-homescreen` application.

### agl-ivi-demo-html5

HTML5 based IVI demo image, using a combination of the Chromium Embedded Framework (CEF) and Web Application Manager (WAM).

### agl-ivi-demo-qt

Qt based IVI demo image, using `homescreen` and `launcher` as well as other Qt applications.

### agl-ivi-demo-qt-crosssdk

This is the corresponding SDK image for `agl-ivi-demo-qt`.

### agl-ivi-demo-control-panel

IVI demo control panel image.  Simple image that runs the `agl-demo-control-panel` application for driving demo setups.

### agl-cluster-demo-flutter

Flutter based IC demo image, using `flutter-cluster-dashboard` application.

### agl-cluster-demo-qt

Qt based IC demo image, using `cluster-dashboard` application.

### agl-gateway-demo

Gateway demo image.  The image runs the KUKSA.val databroker and the associated `kuksa-dbc-feeder` for CAN input.

### agl-telematics-demo

Telematics demo image.  Currently a WIP.

## 2. Preconfigured Demo Images

These images are also available when the `agl-demo` feature is given to `aglsetup.sh`, but are intended for use with a more complete demo setup along the lines of what the project showcases at tradeshows such as CES or Embedded World.  The resulting images use DHCP for network configuration, but the following address assignments are assumed:

- IVI board has address 192.168.10.2
- IC board has address 192.168.10.3
- Gateway board has address 192.168.10.4

Whether an image includes the KUKSA.val databroker or not will be indicated below.

Note that the preconfigured IVI images are for the most part only tested on the Renesas H3ULCB based AGL reference hardware.  While the IC images are typically run on a Raspberry Pi 4 when a standalone IC is used as part of a tradeshow demo, the IC images are simpler and should be a bit more flexible with respect to target platform.

### agl-ivi-demo-flutter-preconfigured

Flutter IVI demo image with:

- IC navigation streaming support
- KUKSA.val databroker present, preconfigured IC images assume it runs on IVI board.
- udev configuration for USB I2C adapter for demo setup LEDs and RTC
- Weston Terminal application removed

### agl-ivi-demo-flutter-preconfigured-gateway

Flutter IVI demo image with:

- IC navigation streaming support
- KUKSA.val databroker removed, as it runs on the gateway.
- Application configuration changes for databroker being on the gateway.
- udev configuration for USB I2C adapter for demo setup LEDs and RTC
- Weston Terminal application removed

### agl-ivi-demo-qt-preconfigured

Qt IVI demo image with:

- IC navigation streaming support
- KUKSA.val databroker present, preconfigured IC images assume it runs on IVI board.
- udev configuration for USB I2C adapter for demo setup LEDs and RTC
- Weston Terminal application removed

### agl-ivi-demo-control-panel-preconfigured

IVI demo control panel image with:

- Default configuration for databroker running on IVI board.

### agl-ivi-demo-control-panel-preconfigured-gateway

IVI demo control panel image with:

- KUKSA.val databroker present, with `kuksa-dbc-feeder` configuration to output CAN messages for the signals coming from the application's vehicle simulation.
- Default configuration disables HVAC and steering wheel pages, as those are driven by hardware connected to the gateway in the full demo setup.

### agl-cluster-demo-flutter-preconfigured

Flutter IC demo image with:

- KUKSA.val databroker removed, as it runs on the IVI board.
- Application configuration changes for databroker being on the IVI board.
- Configuration tweaks to invert the screen orientation, as the IC screen in the full demo setup is upside down.

### agl-cluster-demo-flutter-preconfigured-gateway

Flutter IC demo image with:

- KUKSA.val databroker removed, as it runs on the gateway.
- Application configuration changes for databroker being on the gateway.
- Configuration tweaks to invert the screen orientation, as the IC screen in the full demo setup is upside down.

### agl-cluster-demo-qt-preconfigured

Qt IC demo image with:

- KUKSA.val databroker removed, as it runs on the IVI board.
- Application configuration changes for databroker being on the IVI board.
- Configuration tweaks to invert the screen orientation, as the IC screen in the full demo setup is upside down.

### agl-gateway-demo-preconfigured

Gateway demo image with:

- `kuksa-dbc-feeder` against `can0` interface configured for vehicle simulation (e.g. vehicle and engine speed) CAN messages coming from the `agl-ivi-demo-control-panel-preconfigured-gateway` image.
- A second instance of `kuksa-dbc-feeder` against `can1` interface configured for input and output of CAN messages from and to the steering wheel and HVAC hardware, respectively.