---
title: "BMS"
weight: 31
---

# BMS

How to setup BMS with siliXcon controler. 

Supported BMS:
- [siliXcon BMS](#silixcon-bms)
    - In all FW versions
- [Akuenergy BMS](#akuenergy-bms)
    - Only in VECTOR_LYNX_bms od BLDC_LYNX_bms version
- [Jikong BMS](#jikong-bms)
    - Only in VECTOR_LYNX_bms od BLDC_LYNX_bms version

If your BMS is not listed here please contact our service support.




## Preparation

All BMSs are connected over CAN, setup same CAN speed on the BMS and the controller. Also do not forget CAN termination.  
Default controller CAN speed is 1 mbps.  
If you need change controller CAN speed check this: [`msgconf`](../../lib/_YOS/doc/external/shell_commands.md#msgconf--y-interface-cfg0-cfg1-cfg2-cfg3)


## General settings for all BMS
These settings are common for all BMSs.

### `/bms/bmstype`

Using this parameter setup your BMS type

| bmstype | value
| --- | --- |
| 0 | No BMS on CAN |
| 1 | [siliXcon BMS](https://doc.silixcon.cloud/hw/doc/latest/BMS/) (all big and small bms) |
| 10 | Kortanek BMS |
| 20 | Jigong BMS |
| 30 | Akuenergy BMS |

### `/bms/bmsopts`

> This parameter is only usend when `bmstype` is not a zero.
> It is bitewise parameter

| bmsopts | value
| --- | --- |
| 1 | The BMS is requred to run the motor. If the BMS is not present on CAN, the controller will got to the stop. |
| 3 | STOP vehicle after BMS is disconnected. (This will allow vehicle motion without BMS, but once the BMS is on the CAN it is required) |
| 4 | Disable limitation from the BMS. BMS can be required to run the vehicle, but no limitation from bms will be take in an action|
| 8 | Do not use BMS SOC reading and use BEST (Battery ESTimator inside the controler) |

## siliXcon BMS

`bmsopts` = 1 

SiliXcon BMS is sending all necesary information and no other setup is needed.

## Akuenergy BMS
`bmsopts` = 30 

This is BMS provided by Akuenergy company. This BMS must have newer firmware, that is streaming CAN messages with ID 0x550 -0x559.

If internal humidity sensor is present and humidity is greather than 80%, battery current is limited to 10% and controller will continuously beeping.

> This BMS use controler to set limitation. Battery temperature and cell voltage limiter is used.  
> Also it use "alerts" from the BMS to decrease battery current

### Example confiuration for 13S battery

Apart from your controler configuration set these parameters to this value:

#### Temperature limitation
- `/driver/limiter/btempmaxhi` = 65
- `/driver/limiter/btempmaxlo` = 55
- `/driver/limiter/btempminlo` = 5
- `/driver/limiter/btempminhi` = -1

#### Cell voltage limitation
- `/driver/limiter/bcellmin` = 3.0
- `/driver/limiter/bcellmax` = 4.2

#### Pack voltage limitation
- `/driver/limiter/ubmin` = 39
- `/driver/limiter/ubmax` = 54.6

[More about these parameters](../../_ESC/doc/external/driver/variables/variables_limiter.md#battery-temperature-limitation)

## Jikong BMS
`bmsopts` = 30 

This BMS streaming cell voltages and temperatures. Nothing else is used.  

**WARNING:** This BMS is one port BMS. If BMS decide to switch off during motor run, it can damage the controller. This can happend even if you set all parameters properly inside the controller.

> This BMS use controler to set limitation. Battery temperature and cell voltage limiter is used.  

Set these parameters according to your BMS settings...

#### Temperature limitation
- `/driver/limiter/btempmaxhi` 
- `/driver/limiter/btempmaxlo` 
- `/driver/limiter/btempminlo` 
- `/driver/limiter/btempminhi` 

#### Cell voltage limitation
- `/driver/limiter/bcellmin`
- `/driver/limiter/bcellmax`

[More about the parameters](../../_ESC/doc/external/driver/variables/variables_limiter.md#battery-temperature-limitation)
