# The CAN signal specification for the AGL virtual car.

[Top](./agl-vcar.md)

## Vehicle signal

### ID: 1001 , Vehicle_Status_1

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| PT_VehicleSpeed | unsigned | big | 7 | 15 | 0.015625 | +0 | 0 | 511.984375 | km/h | Current vehicle speed. |

Physical value = 0.015625 * signal value + 0

#### Formula

VehicleSpeed (km/h) = ((payload[0] * 128) + ((payload[1] / 2) & 0x7f)) * 0.015625

#### Supported demo

+ agl-ivi-demo with kuksa.val
+ agl-instrument-cluster standalone/container

<br><br>

### ID: 985 , Vehicle_Status_2

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| PT_FuelLevelLow | unsigned | little | 55 | 1 | 1 | +0 | 0 | 1 | Low or not | Fuel level is low or not. |
| PT_EngineSpeed | unsigned | big | 23 | 16 | 0.25 | +0 | 0 | 16383.75 | rpm | Engine RPM. |
| PT_FuelLevelPct | unsigned | little | 8 | 8 | 0.392157 | +0 | 0 | 100 | % | A percentage of current fuel level. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|Low or not| 1: Fuel level is low, 0: Fuel level is not low. |

Physical engine speed value = 0.25 * signal value + 0

Physical fuel level percent = (signal value * 255 / 100) + 0

#### Formula

EngineSpeed (rpm) = ((payload[2] * 256) + ((payload[3]) & 0xff)) / 4

FuelLevelPct (%) = payload[1] * 255 / 100

#### Supported demo

+ agl-ivi-demo with kuksa.val
+ agl-instrument-cluster standalone/container

<br><br>

### ID: 986 , Vehicle_Status_3

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| PT_RightTurnOn | unsigned | little | 2 | 1 | 1 | +0 | 0 | 1 | ON/OFF | Turn on right or not. |
| PT_LeftTurnOn | unsigned | little | 1 | 1 | 1 | +0 | 0 | 1 | ON/OFF | Turn on left or not. |
| PT_HazardOn | unsigned | little | 0 | 1 | 1 | +0 | 0 | 1 | Active/Inactive | Hazard ON or OFF |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|ON/OFF| 1: ON, 0: OFF |
|Active/Inactive| 1: Active, 0: Inactive |

#### Supported demo

+ agl-ivi-demo with kuksa.val
+ agl-instrument-cluster standalone/container

<br><br>

### ID: 180 , EV_Powertrain

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| EV_ShiftPosition | unsigned | big | 12 | 3 | 1 | +0 | 0 | 7 | Shift Position | Shift Position. |
| EV_MotorFaultLamp | unsigned | big | 1 | 1 | 1 | +0 | 0 | 1 | ON/OFF | Motor fault lamp is light on or off. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|Shift Position| 7: Reserve, 6: Reserve, 5: B, 4: D, 3: N, 2: R, 1: P, 0: Invalid |
|ON/OFF| 1: ON, 0: OFF |

#### Formula

EV_ShiftPosition = (payload[1] >> 2) & 0x07U

#### Supported demo

+ agl-instrument-cluster standalone/container

<br><br>

### ID: 184 , ESP

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| EspOffLamp | unsigned | big | 31 | 1 | 1 | +0 | 0 | 1 | ON/OFF | ESP off warning is on or off. |
| AbsFaultLamp | unsigned | big | 27 | 1 | 1 | +0 | 0 | 1 | ON/OFF | ABS fault lamp is on or off. |
| EspActLamp | unsigned | big | 6 | 1 | 1 | +0 | 0 | 1 | ON/OFF | ESP Act lamp is on or off. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|ON/OFF| 1: ON, 0: OFF |

#### Formula

EspOffLamp = (payload[3] >> 7) & 0x01U

AbsFaultLamp = (payload[3] >> 3) & 0x01U

EspActLamp = (payload[0] >> 6) & 0x01U


#### Supported demo

+ agl-instrument-cluster standalone/container

<br><br>

### ID: 192 , EPS_STATUS

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| EpsFaultLamp | unsigned | big | 6 | 1 | 1 | +0 | 0 | 1 | ON/OFF | EPS fault lamp is on or off. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|ON/OFF| 1: ON, 0: OFF |

#### Formula

EpsFaultLamp = (payload[0] >> 6) & 0x01U

#### Supported demo

+ agl-instrument-cluster standalone/container

<br><br>
