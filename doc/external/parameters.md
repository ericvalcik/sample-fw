---
title: "Parameters"
weight: 12
---

# Parameters

List of all yOS parameters and states.

## Root

| Parameter  | Description                                                        | Range                 |
| ----------- | ------------------------------------------------------------------ | --------------------- |
| drvmode     | Control mode used for acceleration  | 2 - CRT <br> 3 - VLT+FREWHEELING <br> 6 - TRQ        |
| drvopts     | Function options for drivetrain (options can be combined) | 1 - Remove arming sound when brake is pressed <br> 2 - Enable throttle braking <br> 4 - Enable throttle/brake fusion (brake is substracted from throttle) <br> 12 - Disable reversing if throttle/brake fusion is used |
| gearthr     | Gear ratio between driven wheel and motor                                                    | [-]                   |
| odothr      | No. of driven wheel revolution per 1 km (1000/wheel circumference)     |        [-]               |
| safetyopts  | Function options for safety block (options can be combined)     | 1 - Acceleration is disabled until restart if throttle signal is invalid <br> 2- Acceleration is disabled temporary if brake signal is invalid <br> 4 - Controller goes to the permanent BRAKE mode if brake signal is invalid|


| State variable | Description                                                        | Unit          |
| -------------- | ------------------------------------------------------------------ | ------------- |
| armed_state    | Status of the safety block      |   0 - Motor is armed <br> >0 - time [ms] to have motor armed <br> -1 - Disarmed until the next restart             |
| cadence         | Debuging output from Tachopas block       | [-]           |
| limiter_speed   | Current speed of the external speed sensor (if limiter is active)      | [km/h]        |
| mode           | Indicates current state of the state machine (see mode)            | [-]           |
| sig_acc          | Debuging signal for acceleration | [-]           |
| sig_brake       | Debuging signal for braking       | [-] |
| sig_cruise      | Debuging signal for criuse      | [-]           |
| sig_pas      | Debuging signal for PAS cadence (after Cscpas) | [-]           |
| sig_torque     | Debuging signal for PAS torque (after Asctorque) | [-]           |
| speed    | Current speed of the vehicle | [km/h]           |

## Acc

Throttle and clutch signals folder

| Parameter  | Description                                                        | Range                 |
| ----------- | ------------------------------------------------------------------ | --------------------- |
| dual_err    | Settings for throttle checking function  | 0:1 - Threshold for valuation of error <br> -1 - Enables check by throttle end switch    |

| State variable | Description                                                        | Unit          |
| -------------- | ------------------------------------------------------------------ | ------------- |
| acclvl    | Throttle signal limit (taken from power maps)   | [-] |

### Asc

ASC for throttle 1 analogue input.

### Asc_2

ASC for throttle 2 analogue input.

### Ascclt

ASC for clutch analogue input.

### Csc

CSC for throttle normalized signal.

### Cscclt

CSC for clutch normalized signal.

## Best

Battery state-of-charge estimator folder

## Brake

Brake signals folder

| Parameter  | Description                                                        | Range                 |
| ----------- | ------------------------------------------------------------------ | --------------------- |
| comlvlspeedhi   | Enables and defines high threshold for speed dependent throttle braking (see pic)  | 0 - Function turned Off <br> >0 - Speed threshold [km/h]  |
| comlvlspeedlo   | Defines low threshold for speed dependent throttle braking (see pic)   | [km/h]  |
| rgnsldn   |  Defines Static brake disengaging rate   | see LPF [-]   |
| rgnslup   |  Defines Static brake engaging rate   | see LPF [-]  |




| State variable | Description                                                        | Unit          |
| -------------- | ------------------------------------------------------------------ | ------------- |
| comlvl    |  Indicates throttle braking reference | 0:1 [-] |
| sbrakelvl    |  Indicates static brake reference | 0:1 [-] |
| sig_combrake   |  Indicates current throttle braking level | 0:1 [-] |
| sig_dbrake   |  Indicates current dynamic brake level | 0:1 [-] |
| sig_pasbrake   |  Indicates current negative cadence braking level | 0:1 [-] |
| sig_sbrake   |  Indicates current static brake level | 0:1 [-] |
### [ASC](./../../lib/_LIB/doc/external/asc.md)

ASC for dynamic brake analogue input.

### [CSC](./../../lib/_LIB/doc/external/csc.md)

CSC for dynamic brake signal.



## Cruise

Cruise options folder

| Parameter  | Description                                                        | Range                 |
| ----------- | ------------------------------------------------------------------ | --------------------- |
| crsopts     | Cruise options (options can be combined) | 1 - Walk function is enabled <br> 2 - Cruise is enabled (currently disabled) <br> 4 - Activation of throttle ends cruise        |
| walklvl     | voltage command used when entering walk function | 0:1 [-] |



## IO

Input signals mapping folder

Configurable parameter is a array of two values. First value defines ID of the device input. Second byte defines the device and input pin configuration (it is recomended to keep it in default).

[Input ID documentation](./../../_ESCapi/doc/external/cmio_api/common_input_id.md)
> Analog input ID = GPIO, AIN
> Digital input ID = GDIN, DIN


| Parameter  | Description                                              | Input type        |
| ----------- | ------------------------------------------------------- | --------------------- |
| in_acc    | Defines what physical input is mapped as throttle 1 signal       | Analog input ID |
| in_acc_2  | Defines what physical input is mapped as throttle 2 signal       | Analog input ID for 2 chanel throttle<br> Digital input ID for endstop switch |
| in_cadence  | Defines what physical input is mapped as PAS cadence      | Digital input ID |
| in_clutch  | Defines what physical input is mapped as clutch signal     | Analog input ID |
| in_cruise  | Defines what physical input is mapped as cruise activation signal   | Digital input |
| in_dbrake  | Defines what physical input is mapped as dynamic brake signal   | Analog input ID |
| in_light  | Defines what physical input is mapped as light activation signal   | Digital input |
| in_maplock  | Defines what physical input is mapped as second map set activation signal   | Digital input |
| in_mapswitch  | Defines what physical input is mapped as map change activation signal   | Digital input |
| in_reverse  | Defines what physical input is mapped as reverse activation signal   | Digital input |
| in_sbrake  | Defines what physical input is mapped as static brake activation signal   | Digital input |
| in_speed  | Defines what physical input is mapped as external speed sensor  | Analog input ID for SX,SL,SC <br> Digital input ID for AX, AM |
| in_torque  | Defines what physical input is mapped as PAS torque  | Analog input ID |

## Maps

User maps folder

| Parameter  | Description                                              | Range                 |
| ----------- | ------------------------------------------------------- | --------------------- |
| initmap    | Defines what map is used after start-up       | [-] |
| kphlimit  | Defines global speed limit for all maps       | [km/h] |
| mapchlpf  | Low Pass Filter applied after change of map      | check LPF article |
| mapcnt  | Defines count of selectable maps    | [-] |
| maplut  | Defines Look Up Table for maps   | It is 10 position array <br> Position in the array defines map index <br> Value in the array defines GDIN input value |
| mapopts  | Defines count of selectable maps (options can be combined)   | 1 - Maps are changed on level <br> 2 - Allow only the second set of maps <br> 4 - Enables motor disarming during map change |
| pwrlimit  | Defines global power limit for all maps   | [W] |
| reservelevel  | Defines battery SOC level that activates reserve map   | >0 - defines volatage threshold in V <br> <0 - Defines SOC threshold in %|
| reservemap | Defines map which is used for reserve map  | [-] |
| restmapcnt | Defines count of selectable maps from the second map set  | [-] |

| State variable | Description                                                        | Unit          |
| -------------- | ------------------------------------------------------------------ | ------------- |
| map   | Indicates index of currently used map   | [-] |

### MapX

Parameters for particular map

| Parameter  | Description                                              | Range                 |
| ----------- | ------------------------------------------------------- | --------------------- |
| acclvl   | Defines throttle signal multiplier  | -1:1 [-] |
| comlvl  | Defines thrittle brake signal multiplier  | 0:1 [-] |
| kph  | Defines speed limiter | [kph]* |
| paslvl | Defines PAS cadence multiplier | 0:1 [-] |
| pastrq  | Defines PAS torque multiplier | 0:1 [-] |
| pwr  | Defines power limiter   | [W] |
| sbrakelvl  | Defines static brake level   | 0:1 [-] |
| trqlvl  | Defines IREF (motor torque) multiplier| 0:1 [-] |

*speed reading depends on proper setting of other parameters (odothr,gearthr,driver/motor/pp)

### Revmap

Parameters for special map, that is activated by in_revers input. It can be used as a reverse or boost map. For parameters meaning check Mapx folder

## Misc

General settings of the device

| Parameter  | Description                                              | Range                 |
| ----------- | ------------------------------------------------------- | --------------------- |
| beepopts    | Denifes behaviour of sound signalization (options can be combined)  | 1 - Enables arming sound <br> 2 - Enables intro sound <br> 4 - Enables disarming sound <br> 8 - Enables sound SOC indication <br> 16 - Enables throttle error sound signalization <br> 32 - Enambles change map sound signalization <br> 64 - Enables shut down sound signalization <br> 128 - Enables lock sound signalization  |
| dispmode  | Defines UART device selection       | -1 - Automatic selector <br> 0 - Disabled <br> 1 - Bafang <br> 32 - WS LED|
| ledbright  | Defines brightness of WS LED      | 0:100 [%] |
| ledopts  | Defines settings of WS LED indication options can be combined)    | 0-2 - LED colour order <br> 4 - LED is turned OFF while driving <br> 8 - LED reduce brightness while riding|
| lightopts  | Defines settings of vehicle lights | 1 - Turns on lights after start <br> 2 - Enables brake lights |
| shdntim  | Defines timer for shutting down of the device (if the device is idling)   | [s] |
| speedopts  | Defines speed limiter settings   | 1 - Enables aditional speed limiter from external sensor <br> 2 - Set aditional speed sensor only to global speed limiter <br> 128 - Disables primary speed limiter (from motor)|
| syncmode  | Defines Master-Slave configuration  | 0 - Function disabled <br> 1 - Device is set as master (must be device with addr = 0) <br> 2 - Device is set as slave|
| warntim | Defines timer for acoustic signalization (energized state)  | [s] |

### [PAS](./../../lib/_LIB/doc/external/pas.md)

General settings of the device

| Parameter  | Description                                              | Range                 |
| ----------- | ------------------------------------------------------- | --------------------- |
| pasopts    | PAS options (options can be combined)  | 1 - Enables reverse pedaling brake (PAS brake) |
| trqgain   | Defines torque multiplier (for human_trq calculation)  | [-] |

| State variable | Description                                                        | Unit          |
| -------------- | ------------------------------------------------------------------ | ------------- |
| human_trq    | Indicates torque signal for human watts calculation | [-] |
| human_watts    |  Indicates human watts signal (cadence x human_trq x 10) | [-] |
| paslvl   |  Indicates value of paslvl multimplier | [-] |

### Asctorque

[ASC](./../../lib/_LIB/doc/external/asc.md) for PAS torque analogue input.

### Ascwatts

[ASC](./../../lib/_LIB/doc/external/asc.md) for Human watts signal

### Cscpas

[CSC](./../../lib/_LIB/doc/external/csc.md) for PAS cadence signal.

### Cscpasbrake

[CSC](./../../lib/_LIB/doc/external/csc.md) for negative PAS cadence signal - braking (if activated).


### Tachopas

[Tacho block](./../../lib/_LIB/doc/external/tachopas.md) for cadence input signal

## Tachospd

[Tacho block](./../../lib/_LIB/doc/external/tachopas.md) for external speed sensor input signal.



## Other system folders


### Driver

Information about the driver.
[Driver documentation here](./../../_ESC/doc/external/driver/)


### Common

Information about I/O of the device
[Common block documentation here](./../../_ESC/doc/external/common/)
