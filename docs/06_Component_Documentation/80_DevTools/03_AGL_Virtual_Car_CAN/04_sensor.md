# The CAN signal specification for the AGL virtual car.

[Top](./agl-vcar.md)

## Sensor

### ID: 33 , Steering Wheel

8 Byte CAN message.

#### Signals
| Name | Type | Endian | Start Bit | Bits Length | Factor | Offset | Min | Max | Unit | Comment |
|:-----|:-----|:-------|----------:|------------:|-------:|-------:|----:|----:|-----:|:------- |
| SW_CruiseEnable | unsigned | little | 55 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise enable switch of the steering wheel. |
| SW_CruiseResume | unsigned | little | 54 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise resume switch of the steering wheel. |
| SW_CruiseSet | unsigned | little | 52 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise set switch of the steering wheel. |
| SW_CruiseCancel | unsigned | little | 51 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise cancel switch of the steering wheel. |
| SW_CruiseLimit | unsigned | little | 49 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise limit switch of the steering wheel. |
| SW_CruiseDistance | unsigned | little | 48 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A cruise distance switch of the steering wheel. |
| SW_Horn | unsigned | little | 63 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A horn switch of the steering wheel. |
| SW_LaneDepartureWarning | unsigned | little | 56 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A lane departure warning switch of the steering wheel. |
| SW_Voice | unsigned | little | 42 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A voice recolonization switch of the steering wheel. |
| SW_PhoneHangup | unsigned | little | 41 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A phone hangup switch of the steering wheel. |
| SW_PhoneCall | unsigned | little | 40 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A phone call switch of the steering wheel. |
| SW_Previous | unsigned | little | 39 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A previous switch of the steering wheel. |
| SW_VolumeUp | unsigned | little | 38 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A volume up switch of the steering wheel. |
| SW_Mode | unsigned | little | 37 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A mode change switch of the steering wheel. |
| SW_VolumeDown | unsigned | little | 36 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A volume down switch of the steering wheel. |
| SW_Next | unsigned | little | 35 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A next switch of the steering wheel. |
| SW_Info | unsigned | little | 33 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A information switch of the steering wheel. |
| SW_VolumeMute | unsigned | little | 32 | 1 | 1 | +0 | 0 | 1 | ON/OFF | A volume mute switch of the steering wheel. |

#### Units

| Unit | Descriptions |
|:-----|:-------------|
|ON/OFF| 1: ON, 0: OFF |

#### Supported demo

+ agl-ivi-demo with kuksa.val

<br><br>
