# The CAN signal specification for the AGL virtual car.

[Top](./agl-vcar.md)

## Body

### ID: 142 , Body control module status 1

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| RightSeatBelt | unsigned | big | 13 | 1 | 1 | +0 | 0 | 1 | Fasten/Open | Fastened or not to right seat belt in front side. |
| LeftSeatBelt | unsigned | big | 12 | 1 | 1 | +0 | 0 | 1 | Fasten/Open | Fastened or not to left seat belt in front side. |
| RearLeftDoor | unsigned | big | 7 | 1 | 1 | +0 | 0 | 1 | Open/Close | Rear left side door now open or close. |
| RearRightDoor | unsigned | big | 6 | 1 | 1 | +0 | 0 | 1 | Open/Close | Rear right side door now open or close.|
| FrontRightDoor | unsigned | big | 5 | 1 | 1 | +0 | 0 | 1 | Open/Close | Front right side door now open or close. |
| FrontLeftDoor | unsigned | big | 4 | 1 | 1 | +0 | 0 | 1 | Open/Close | Front left side door now open or close. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|Fasten/Open| 1: Fasten, 0: Open |
|Open/Close | 1: Open, 0: Close |

#### Supported demo

+ agl-instrument-cluster standalone/container

<br><br>

### ID: 143 , Body control module status 2

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| ImmobilizerFaultLamp | unsigned | big | 4 | 1 | 1 | +0 | 0 | 1 | ON/OFF | Immobilizer fault lamp is light on or off. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|ON/OFF| 1: ON, 0: OFF |

#### Supported demo

+ agl-instrument-cluster standalone/container

<br><br>

### ID: 48 , HVAC_Control_1

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| PT_TempLeft | unsigned | little | 7 | 8 | 0.4166666667 | +0 | 0 | 100 | C | Temperature of the air conditioner for left side. |
| PT_TempRight | unsigned | little | 15 | 8 | 0.4166666667 | +0 | 0 | 100 | C | Temperature of the air conditioner for right side. |
| PT_FanSpeed | unsigned | little | 39 | 8 | 0.392157 | +0 | 0 | 100 | % | Fan rotation rate. |

#### Units

Physical temp left value = 0.4166666667 * signal value + 0

Physical temp right value = 0.4166666667 * signal value + 0

Physical fan speed = 0.392157 * signal value + 0

#### Formula

PT_TempLeft (C) = payload[0] * 5 / 12

PT_TempLeft (C) = payload[1] * 5 / 12

PT_FanSpeed (%) = payload[4] * 20 / 51

#### Supported demo

+ agl-ivi-demo with kuksa.val
