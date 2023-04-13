---
title: "Modes"
weight: 10
---
# Application MODEs description


| MODE name | MODE number | Description |
| ---------- | ----------------- | ----------------- |
| **MODE_STOP** | 0 | Driver or common block is in error. Check /driver/error and /common/error.    |
| MODE_INVALID_INPUTS | 2 | Controler GPIO reading is not valid. |
| MODE_BMS_ERROR | 6 | Connected BMS is in error |
| MODE_IDENT | 9 | Motor identification is in process |
| MODE_INIT | 10 | LYNX is initializing. This equals to BEST reset_time |
| MODE_CAN_TIMEOUT | 20 | Timeout for msg 0x5FF (if used). Timeout is 200ms |
| MODE_WELDING | 30 | Special mode. Controller is used as welder. |
| MODE_CHARGE_EXT_FOREVER | 65 | BMS report charging, do not allow ride | 
| MODE_CHARGE_EXT | 68 | BMS report charging, do not allow ride |
| MODE_CHARGE| 70 | Controller act as step-up converter and charging battery.   |
| MODE_SLAVE | 79 | Device is under the control from the Master controller (Multi controller setup)   |
| MODE_OVR| 80 | Driver is overriden by Driver API |
| MODE_LOCK| 90 | Device is locked   |
| MODE_STANDBY | 100    | Device is ready, motor is freewheeling |
| MODE_BRAKE | 101 | Device is braking |
| MODE_ACCELERATE | 102 | Device is accelerating from throttle |
| MODE_KICKBACK | 104 | Kickback assist active |
| MODE_ASSIST| 105 | Device is accelerating from PAS     |
| MODE_CRUISE| 110 | Device is accelerating due to cruise function   |
| MODE_DISARMED_FOREVER | 120 | LYNX disarmed forever |
