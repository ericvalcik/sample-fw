---
title: "OBD2"
weight: 30
---

# OBD2

> OBD2 is paid feature. It is not present in standard LYNX firmware.  
> To get OBD2 conntact our service support.


## DTC codes

| DTC | Description | LYNX implementation notes |
| --- | --- | --- |
| P0120 | Throttle/Pedal Position Sensor/Switch "A" Circuit |  Output of /acc/csc is NaN |
| P0122 | Throttle/Pedal Position Sensor/Switch "A" Circuit Low | Param /acc/asc/absmin must be non zero. <br> /acc/acc input is below absmin <br> Or same for /acc/asc_2.|
| P0123 | Throttle/Pedal Position Sensor/Switch "A" Circuit High | Param /acc/asc/absmax must be non zero. <br> /acc/acc input is above absmax <br> Or same for /acc/asc_2.| 
| P0500 | Vehicle Speed Sensor "A"  | Reserved (not implemented)
| P0A1B | Drive Motor "A" Control Module | /driver/error is non zero |
| P0A2A | Drive Motor "A" Temperature Sensor Circuit | Motor sensor temperature fail. <br> Trigger if RThermistor is greather than 50 kOhm <br> Or if RThermistor is lower than 80 Ohm |
| P0A2F | Drive Motor "A" Over Temperature | Motor overtemperature - /driver/limit & 256 <br> Motor sensor must be present and properly configured |
| P0A3C | Drive Motor "A" Inverter Over Temperature | Driver over temperature condition - /driver/stat & 4 |
| P0A3F | Drive Motor "A" Position Sensor Circuit | There is motor sensor error - /driver/stat & 16 |
| P0C05 | Drive Motor "A" Phase U-V-W Circuit/Open | Disconnected motor |
| P0A9B | Hybrid Battery Temperature Sensor "A" Circuit | Active, when CAN BMS lost connection |





## PID codes


| PID | Description |
| --- | --- |
| 0x05 | Controler temperature [°C +40]
| 0x8D | Motor RPM [rpm * 100]
| 0x5B | Battery SOC [%]
