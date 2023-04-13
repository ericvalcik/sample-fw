---
title: "Messages"
weight: 11
---

# CAN msg - Sending

All here listed messges are **little endian**.

## 0x600 - LYNX status
Send every 100ms.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

| Byte   | type        |    Description             |
| :---   |  :---        |       :---         | 
| 0   |  UINT_8   |   LYNX ID = 10           |
| 1   |  UINT_8    |   LYNX mode               |
| 2   |  UINT_8    |   Current power map      |
| 3   |  UINT_8    |   [Driver status word](./../../_ESC/doc/external/driver/driver_state.md) |
| 4-5 |  UINT_16    |   [Driver limit word](./../../_ESC/doc/external/driver/variables/variables_limiter.md#limit)   |
| 6-7 |  UINT_16    |   [Driver error word](./../../_ESC/doc/external/driver/driver_error.md)   |


## 0x610 - Motor status
Send every 100ms.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

| Byte   | type        |    Description             |
| :---   |  :---        |       :---         | 
| 0-1   |  INT_16   |   Motor current [A] (q axis) |
| 2-3   |  INT_16    |   Motor speed [rpm]               |
| 4-5   |  INT_16    |   Vehicle speeed [km/h]      |
| 6-7   |  INT_16    |   Motor power [W] |


## 0x618 - Battery status
Send every 100ms.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0   | -   |   Bit 0 - SOC/voltage below reserve level     |
| 1   | -   |   Map type: <br> 1 - Normal <br> 2 - Restricted <br> 3 - reverse <br> 4 - boost <br> 5 - Reserve map     |
| 2   |  UINT_8    |   SOC [%]  |
| 3   |  UINT_8    |   SOH [%] (Not implemented, reserved) |
| 4-5   |  INT_16    |   Battery voltage [V / 100]  |
| 6-7   |  INT_16    |   Battery current [A / 50]  |


## 0x620 - ODO and TRIP
Send every 100ms.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

> **ATTENTION** - Breaking change
> From LYNX 3.x this message send [kph] not [motor revolutions]

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0-3   | UINT_32   |   TRIP [0.01 km ], distance counter. Can by reset from display or shell cmd [`tripreset`](./shell_commands.md#tripreset)    |
| 4-7   |  UINT_32    |  ODO [0.1 km], total distance counter |

## 0x625 - Range estimator
Send every 800ms

TBT

## 0x626 - Relative values 

> Mostly used by display for bargrafs

Send every 250.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0-1  |  INT_16    |   Relative motor phase current -32767 - 32767. (32767 = iref)  |
| 2-3   |  INT_16    |   Driver total limit. 32767 = Full power <br> 0 = Zero power (100% limitation)  |
| 4-5 | INT_16 | Relative speed (-323767 - 32767). (Parameter /maps/maxkph = 32767)
| 6-7 | | Reserved |

## 0x627 
Send every 500.
Send only if device addr is 0. (Device is MASTER in multicontroller setup)

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0-1  |  INT_16    |   Human wats. [0.1*W] |
| 2-7 | | Reserved |


# CAN msg - Receive

## 0x5FF - CAN input message

Using this message, you can send inputs to LYNX over CAN.  

Once the message is present on CAN, it is required. Otherwise LYNX goes to MODE = 20 - CAN timeout.  
Timetout for this message is 200ms.

> 32767 = invalid reading (for INT_16 values)
> Use this value, when one of your CAN input is invalid. Or stop sending the message.

| Byte   | type        |     Description        |   Input parameter value (IO folder) |
| :---   |  :---        | :---       | --- |
| 0-1     |  INT_16  (Little endian)       |   can_level1 | `1,255` |
| 2-3     |   INT_16  (Little endian)      |   can_level2 | `2,255` |
| 4-5     |   INT_16  (Little endian)      |   can_level3 | `3,255` |
| 6     |   UINT_8     |   Bit 0 - CAN digital in 0 <br>  Bit 1 - CAN digital in 1 <br>  Bit 2 - CAN digital in 2 <br>  Bit 3 - CAN digital in 3| `10,255` <br>  `11,255`<br>  `12,255`<br>  `13,255`|
| 7 | UINT_8 | IO cmd. BIT0 - disarm (same as seat switch) | - |



## 0x630 + sender address - Command request

| Byte      | type      |    Description                |
| :---      |  :---     |       :---                    | 
| 0         |  UINT_8   |   Receiver addr, 255 = broadcast       |
| 0-3 bite operation code,<br> 4-7 session id |
| 2         | UINT_8    | Operation code index                  |
| 3         | UINT_8    | Operation code subindex               |
| 4-7       | -         |   Data                                |

## 0x640 + sender address - Command response

| Byte      | type      |    Description                |
| :---      |  :---     |       :---                    | 
| 0         |  UINT_8   |   Receiver addr, 255 = broadcast       |
| 1          |-  |Same as was in command request
| 2         | -     |Same as was in command request |
| 3         | UINT_8    | Message index (for bulk data), 255 = last message              |
| 4-7       | -         |   Data                                |

### Operation code
| Operation code | Description | 
| --- | --- |
| 1 | Read parameter |
| 2 | Write parameter  |
| 3 | Load from backup parameter (restore) |
| 3 | Read metadata |
| 4 | Write metadata (reserved) |
| 5 | Issue command |
| 6 | Load parameters |
#### Read / write / load from backup parameter
When you read parameter, in request msg, leave "data" null.  
When you set parameter, in response is real/set value.

Message index = 255 -> parameter/map not exists.

| Index | Parameter list description |
| --- | --- |
| 0 - 10 | Map number |
| 15 | Map options |
| 16 | Gear opts |
| 20 - 255 | Reserved for other parameters |

| Sub-index for map list| Description | Data |
| --- | --- | --- |
| 1 | kph | Float |
| 2 | pwr | Float |
| 3 | trqlvl | Float |
| 4 | accvlv | Float |
| 5 | paslvl |  Float |
| 6 | pastrq | Float |
| 7 | comlvl | Float |
| 8 | sbrakelvl | Float |

**Index = 15**

> This param are READ ONLY, you can not change them using CAN message

| Sub-index for map options| Description | Data |
| --- | --- | --- |
| 1 | kphlimit | Float |
| 2 | pwrlimit | Float |
| 3 | restmapcnt | UINT8 |
| 4 | mapcnt | UINT8 |

**Index = 16**
| Sub-index for gear options| Description | Data |
| --- | --- | --- |
| 1 | gearthr | Float |
| 2 | odothr | Float |

#### Read / write metadata
When data are char, subindex in response is incremented each msg. Last message is with subindex = 255.

| Index | Description | Data |
| --- | --- | --- |
| 1  | Usrcalib - DevName| Char |
| 2 | Usrcalib - DevSN | Char |
| 3 -12 | Usrcalib - DevIdX | INT_32 |
| 20    | Device SN | 12x Char |
| 21    | SWID | Char |
| 22    | HWID  | Char |
| 30    | SWID hash   | UINT32    |
| 31    | Config hash   | UINT16    |

#### Issue command

| Index | Description | sub-index | Response Data |
| --- | --- | --- | --- |
| 1  | lock device | | 0 - success |
| 2  | un-lock device || 0 - success |
| 3  | Force map | Map number | 0 - success |
| 4  | Reset trip counter | | 0 - success |
| 20 | Parameter save | 0 - succes, 1 - error |
| 21 | Load parameters from main | 0 - success, 1 - error |
| 22 | Load parameters from backup | 0 - success, 1 - error |
| 50 | Execute identrun <br> mode must be STANDBY or BRAKE | 0 - success |


## 0x621  Geofencing, global limitation
Timeout is 10s. After timeout vehicle is disarmed.

> If you set parameter [mapopts](./parameters.md) |= 128, this message is required to run the vehicle. 

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0 - 1  |  -   |   Global limitation, 0xFFFF = no limitation , 0x0 = Full limitation (You have 0 available power)          |
| 2        | UINT8_T   [kph] | Maximumm allowed speed, 255 - no limitation |
| 3 |   UINT8 | Bit 1 - Disarm byke forever |
| 4 - 7| |reserved| 




## 0x600
If device is SLAVE (syncmode = slave). Device is receiving 0x600 msg from MASTER device. Than it is parsing "map" number.¨

> ## 0x680
> Do not use this, deprecated!
>Value:
>- 0x02 - Switch to another power map
>- 0x40 - Reset trip counter

# CAN msg - Receive from BMS siliXcon
This messages are for BMS support. They are present, only of BMS support is added to the FW build.

## 0x500 - BMS status msg
This MSG is from siliXcon BMS. 

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0   |  UINT_8   |   BMS ID == 203          |
| 1   |  UINT_8    |   BMS driver state            |
| 2   |  UINT_8    |   BMS error word - If non zero, do disarm     |
| 3   |  UINT_8    |   SOC, 0-200 = 0-100 [%], 255 = NaN - If valid, is is used instead of BEST |
| 4-5 |  UINT_16    |   BMS limit word   |
| 6 |  UINT_8     |  BMS suggested limit value for positive current (from the battery) 0-255, 255 = full current (no limitation) |
| 7 |  UINT_8    |  BMS suggested limit value for negative current (to the battery) 0-255 |

## 0x506 - BMS recommended limiters settings
This MSG is from siliXcon BMS.  
If value is 0, limiter setting are not changed.

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0-1   |  INT_16   |   Ibpos [A / 10] |
| 2-3   |  INT_16    |   Ibneg [A / 10] |
| 4-5   |  INT_16    |   Ubmin [V / 10] |
| 6-7   |  INT_16    |   Ubmax [V / 10] |


# CAN msg - Receive - BMS kortanek
This messages are for BMS support. They are present, only of BMS support is added to build.

## 0x502 - BMS status msg
This MSG is from siliXcon BMS. 

| Byte   | type        |    Description      |
| :---   |  :---        |       :---         | 
| 0   |  UINT_8   |   SOC [%]  |
| 1   |  UINT_8    |   Temp min [°C - 40] |
| 2   |  UINT_8    |   Temp man [°C - 40] |
| 3-4   |  UINT_16    |   Cel min |
| 5-6   |  UINT_16    |   Cel min |
