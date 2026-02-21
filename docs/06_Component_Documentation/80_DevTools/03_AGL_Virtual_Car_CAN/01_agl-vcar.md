# The CAN signal specification for the AGL virtual car.

## Overview
AGL's virtual car CAN signal specification provides an open, freely reusable set of CAN signals.  It's independent of existing CAN signal specifications.  AGL uses this specification for open-source development.  AGL users will be able to change the source code to adopt internal/confidential CAN signal specifications.

These CAN signal specification based on [AGL Instrument Cluster API specification](https://lf-automotivelinux.atlassian.net/wiki/spaces/IC/pages/17991785/IC-Service+API), [VSS specification](https://github.com/COVESA/vehicle_signal_specification), and passed demo development.

## Details of the AGL virtual car CAN signals

### Message list
| Category | Overview |
|:-----|:-----|
| [Vehicle Signal](./02_vehicle_signal.md) | Vehicle speed, engine rpm, and turning signal. |
| [Body](./03_body.md) | . |
| [Sensor](./04_sensor.md) | Steering sensor, GNSS, and etc. |
