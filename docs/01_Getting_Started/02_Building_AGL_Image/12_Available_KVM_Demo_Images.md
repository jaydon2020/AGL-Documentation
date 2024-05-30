---
title: Available KVM Demo Images
---

## 1. KVM Demo Images

These images are available when the `agl-kvm` feature is given to `aglsetup.sh`.  The resulting images use DHCP for network configuration, but the following address assignments are assumed:

- KVM demo board has internal address 172.16.10.1
- IVI guest has internal address 172.16.10.2
- IC guest has internal address 172.16.10.3

Note that all of the KVM demo images are currently configured for the Renesas H3ULCB based AGL reference hardware.  While the demo can likely be run on other hardware, there are some pieces of configuration that are currently hard-coded that would need to be changed.  One particular aspect of hardware-specific configuration is that the USB connection for the touchscreen used for the IVI needs to be plugged into the lower USB connector of the 2x USB 3.0 connector header on the reference hardware.

### agl-ivi-demo-flutter-guest

Flutter IVI demo image with:

- IC navigation streaming support

### agl-cluster-demo-flutter-guest

Flutter IC demo image with:

- KUKSA.val databroker removed, as it runs on the IVI guest.
- Application configuration changes for databroker being on the IVI guest.

### agl-kvm-demo

Base KVM demo image with:

- `agl-ivi-demo-flutter-guest` IVI guest.
- `agl-cluster-demo-flutter-guest` IC guest.
- KUKSA.val databroker not present, as it runs on the IVI guest.

## 2. Preconfigured KVM demo images

These images are also available when the `agl-kvm` feature is given to `aglsetup.sh`, but are intended for use with a more complete demo setup along the lines of what the project showcases at tradeshows such as CES or Embedded World.  The resulting images use DHCP for network configuration, but the following address assignments are assumed:

- KVM demo board has internal address 172.16.10.1
- IVI guest has internal address 172.16.10.2
- IC guest has internal address 172.16.10.3
- KVM demo board has address 192.168.10.2
- Gateway board has address 192.168.10.4

### agl-ivi-demo-flutter-guest-preconfigured

Flutter IVI demo image with:

- IC navigation streaming support
- KUKSA.val databroker removed, as it runs on the KVM host.
- Application configuration changes for databroker being on the KVM host.
- Platform service daemons such as audio and HVAC support removed as they run on the KVM host.
- Weston Terminal application removed

### agl-ivi-demo-flutter-guest-preconfigured-gateway

Flutter IVI demo image with:

- IC navigation streaming support
- KUKSA.val databroker removed, as it runs on the gateway.
- Application configuration changes for databroker being on the gateway.
- Platform service daemons such as audio and HVAC support removed as they run on the KVM host.
- Weston Terminal application removed

### agl-cluster-demo-flutter-guest-preconfigured

Flutter IC demo image with:

- KUKSA.val databroker removed, as it runs on the KVM host.
- Application configuration changes for databroker being on the KVM host.
- Configuration tweaks to invert the screen orientation, as the IC screen in the full demo setup is upside down.

### agl-cluster-demo-flutter-guest-preconfigured-gateway

Flutter IC demo image with:

- KUKSA.val databroker removed, as it runs on the gateway.
- Application configuration changes for databroker being on the gateway.
- Configuration tweaks to invert the screen orientation, as the IC screen in the full demo setup is upside down.

### agl-kvm-demo-flutter-preconfigured

KVM demo image with:

- `agl-ivi-demo-flutter-guest-preconfigured` IVI guest.
- `agl-cluster-demo-flutter-guest-preconfigured` IC guest.
- KUKSA.val databroker.
- Platform service daemons such as audio and HVAC.

### agl-kvm-demo-flutter-preconfigured-gateway

- `agl-ivi-demo-flutter-guest-preconfigured-gateway` IVI guest.
- `agl-cluster-demo-flutter-guest-preconfigured-gateway` IC guest.
- Platform service daemons such as audio and HVAC
- Service configuration for the KUKSA.val databroker being on the gateway.