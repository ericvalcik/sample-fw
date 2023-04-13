---
title: "Signal_flow"
weight: 14
---
# Main signals flow & other inputs

 The general block overview is shown in the picture below. Physical signals are linked to logic in the Input Mapping block. Then all these signals are processed in the Drive Logic block ( literally, it defines how output reacts to the input ). Drive Logic block is divided into seven subcategories. Processed signals from this block are used for the control of the outputs.

![lynxoverview.jpg](../pics/lynx_overview.jpg)

## Input mapping

Lynx allows mapping its logic inputs to physical inputs on a particular device (usually a controller). This ensures high modifiability of the system in reference to used HW.

## State machine

The system is always persisting in one of its allowed states. These states are also called modes and are visible as a state variable inside the system as a number. An important fact about the state machine is that it gives the order to all the modes - the lower the number is, the higher the priority is. This will ensure that the behavior is always defined  (i.e., throttle and brake applied simultaneously). 

More information can be found in Mode chapter


## Braking

There are four independent sources of braking in Lynx. Each of them can be activated/deactivated and modified by dedicated parameters. The braking input, that has the highest value, is fed to the driver block and used as its **cmd** (driver mode is 8 in this scenario). Signal flow of braking signal is visible on the picture below. All the braking inputs (and their signal flow) are closely described in the upcoming chapters.

![brakegeneral.jpg](../pics/lynx_braking.jpg)

### Static brake

Static brake is a basic braking source. A push button is usually used as input element. Its output (0 or 1) is then multiplied by the value of parameter **sbrakelvl** (this allows to define the maximal braking force of static brake input). There is also possibility to change dynamic of ascending and descending rate of this braking signal by parameters **rgnslup** and **rgnsldn**. Simple signal flow is depicted on the picture below

> This function is activated if in_sbrake is mapped to a valid physical input

![sbrake.jpg](../pics/lynx_braking_static.jpg)

### Dynamic brake

  Proportional analogue value is normalized in ASC block and can be then shaped in CSC block. The output from CSC block is fed directly to the Max value selector (see pic below) - Braking force is corresponding to the voltage level on its input.

  >This function is activated if in_dbrake is mapped to a valid physical input

  > There are also some advanced modes combining throttle & dynamic brake. More information about these is stated in accleration chapter

![dbrake.jpg](../pics/lynx_braking_dynamic.jpg)

### Throttle braking

  ASC for the throttle analogue input is set in the way that its output is negative when the throttle is released. This negative signal is fed through independent signal path (if **comlvl** is > 0) through signal mutliplier **comlvl** (defines braking force) and speed dependent braking block. This block allows to set speed level where the throttle braking is deactivated (**comlvlspeedlo**) and speed level above which is the braking force maximal (**comlvlspeedhi**). The braking force is lineary inrcreasing/decreasing between these two levels (check pictures below). 

  >This function is activated if paramter **comlvl** is not equal to zero

  >Motor will spin in opposite direction if signal from throttle is negative and **comlvl** is set to 0

 ![speedbraking.jpg](../pics/throttle_braking_lynx.jpg)

 General signal flow for throttle braking is depicted here:

 ![tbrake.jpg](../pics/lynx_braking_throttle.jpg)

### Reverse pedaling braking

The system can generate negative signal from reverse pedaling if the tachopas block is set accordingly. Negative signal (cadence) is fed to the cscpasbrake block (if **pasopts** = 1) and can be shaped accordingly in this block. Output from this block is multilied by value defined by **paslvl** parameter (defines braking force). Then, the signal is going to the brake max value selectror. 

> This function is activated if paramter **pasopts** is set to 1

Reverse pedaling signal flow

![pbrake.jpg](../pics/lynx_braking_pas.jpg)

## Acceleration

The system offers advanced features in reference to acceleration, such as throttle/brake fusion, electronic clutch, or selectable motor mode. These features are described in the upcoming rows

Features are dived in sub-chapters to simplify their explanations. But first let`s descibe the common part of the accleration signal flow. (see pic below). Generic throttle input is fed through safety block. This can disable acceleration command if there is a safety hazard (more in coresponding chapter). Then the acceleration signal is multiplied by value of parameter **acclvl** (defines maximal value of acceleration command). Multiplied signal (sig_acc) is going to the state machine. sig_acc is directly transfered as cmd for driver, and driver motor mode is defined by parameter **drvmode** (user can select what motor mode will be used for acceleration)

![throttlegeneral.jpg](../pics/lynx_accel.jpg)

### Throttle

Generic analogue throttle input. The signal is normalized in ASC block, shaped in CSC block and then continue as general throttle signal flow

Throttle signal flow:

![throttle.jpg](../pics/lynx_throttle_simple.jpg)
### Reversing from dynamic brake

Signal from dynamic brake (after CSC) is substracted from throttle signal (after CSC). Both positive and negative signal continues via the same path (as general throttle signal), but negative value of the acceleration signal cause opposite direction of the motor spinning. 

This function can be helpful if there is a need for instant transition to reverse direction

> This function is activated if paramter **drvopts** is set to 4

![throttlereverse.jpg](../pics/lynx_throttle_fusion_reverse.jpg)

### Throttle & brake fusion

The signal path is the same as in the previous mode (dynamic brake is substracted from throttle). The difference is that if the signal is negative, it is used as braking signal and motor starts to brake (braking force is defined by the value of the signal). This is ascting similar as a clutch with the fact that the motor is braking if the resultant signal is below zero

>This function is activated if paramter **drvopts** is set to 12

![throttlefusion.jpg](../pics/lynx_throttle_fusion_brake.jpg)


### E-clutch

This mode can be used as an emulation of the clutch from conventional motorbikes. Analogue signal from clutch input (after CSC) is used as multiplier for throttle signal (after CSC). Then this signal continues through the general accleration signal path

>This function is activated if in_clutch is mapped to a valid physical input

![throttlefusion.jpg](../pics/lynx_throttle_clutch.jpg)

## Pedal Assistant System

Lynx offers advanced pedal assistant system with cadence and torque input with possibility to activate "human watts" feature to fulfil the most demanding users' requirements.

In default state, only the cadence sensor is used (but cadence is implemented in all the other modes). Digital pulses from its input are normalized in Tachopas block and its output is read as cadence (normalized signal 0-1). Cscpas block is there for optional shaping of the cadence signal. The last block before the state machine is a multiplier. It scales cadence signal by value defined by parameter **paslvl** (defines amount of PAS support). To the state machine is fed also information about torque. Its input is value 1 in the case where no torque sensor is mapped to the system. But before it reaches the state machine, the torque signal is multiplied by **pastrq** value (defines torque for PAS suppor). Motor mode 3 (with limit for maximal torque - defined by sig_toruqe) is used for driver. Signal sig_pas defines driver cmd.

>This function is activated if in_cadence is mapped to a valid physical input

PAS default configuration:

![pas.jpg](../pics/lynx_pas.jpg)

### PAS with torque sensor

This is advanced option of PAS where also infromation about force acting on pedals is processed. Raw analogue signal is normalized in asctorque block, then is multiplied by **trqlvl** value (to reduce global motor torque). Resultant signal is then follows the same path as in the default PAS configuration.

>This advanced PAS function is activated if in_torque is mapped to a valid physical input

![pastorque.jpg](../pics/lynx_pas_torque.jpg)

### PAS with human watts 

The latest addition to the PAS features. It is combining cadence and torque signal to achieve better torque control during fast pedalling.

The analogue torque signal path is the same as in the previous section. The signal after **trqlvl** multiplier is fed to another multiplier with the factor defined by **trqgain** (human_trq). Then the output signal is merged with the cadence signal and multiplied by factor of ten. The resultant signal is called human_watts. Its raw value is normalized in ascwatts block. 

The final block of this advanced fusion is Max value selector. It send to its ouput the signal with the higher value (direct torque from torque sensor or human watts). The output signal then follow the same path as in the default PAS configuration

>This advanced PAS function is activated if in_torque is mapped to a valid physical input and trqgain is not equal to zero

![pashw.jpg](../pics/lynx_pas_hw.jpg)

## User Maps

By the term “map”, we refer to an end-user selectable configuration for the drivetrain system. This allows to the user to choose the best settings for the given ride conditions. The configuration is made in the way, that if a particular map is entered, its parameters (defined in the particular map folder) are applied to system as limiting values. This can be seen in the picture below

![usermaps.jpg](../pics/lynx_usermaps.jpg)

### Map types

There are 5 types of maps:
 - **Safety map** (map0): This map is in default configuration loaded directly after start of the system. Its parameters are set in the way, that the ride is not possible, until next map is activated. Map0 is possible to reach only during startup, it is not accessible later during map change. If function of safety map is not required, it is possible to set initializing map to another map, defined by parameter `initmap`.
 - **Normal map**: This type of map is accesible during the normal operating conditions. The system can cycle between these maps without any limitations 
 - **Restricted map**: This type of map is accessible only when `in_maplock` is activated. Transition between  normal / restricted maps can be configured by parameter `mapopts`
 - **Reverse map** (revmap): This map is activated only if `in_reverse` is active. It can be used for entering reverse or boost mode (depends on its parameters). Disarming is actiaved when entering reverse map.
  - **Reserve map**: This map is activated only if the battery SOC (or voltage) is below the value in parameter `reservelevel`. Reverse map can be activated only once per ride. Parameter `reservemap` defines what particular map (must be in range of accesible maps) is used for this functionality.
 
 ### Accesible amount of maps
 The system supports totally up to 9 power maps. Count of the normal map is defined by parameter `mapcnt`. Amount of restricted map corresponds to `restmapcnt`. The total amount of maps is shared between normal and restricted maps (`mapcnt` + `restmapnt` defines total number of accesible maps and must be lower than 9).
 
 There are also one safety map (map0) and one special reverse map (revmap), as addition to these accessible maps (an example is depicted in the picture below)

 ![pashw.jpg](../pics/lynx_mapcount.jpg)

 ### Map change

 In default, maps are changed by pressing of a button. If necessary, it is possible to change map by a voltage level, present on the map switch input. This can done by setting of parameters `mapopts` (reaction on voltage level) and `maplut` (look up table for maps based on the voltage level).

 There is also parameter (`mapchlpf`) for smoothening of the transition during the interchange between two maps.

### Global limiters

There is possibility to setup global maps limiters. This option is availabe for speed (`kphlimit`) and power (`pwrlimit`) limiters. With these parameters sets to a specific value, its corresponing value in a particular map will be always limited to these global parameters

## Safety 

Lynx incorporates several functions for achieving higher safety. 

### Diasrming block

In-built safety system. It can check several inputs and based on its values, the throttle signal path can be disabled (before entering driver) together with the cruise function. This ensures that the motor will not accelerate from the throttle path and cruise is deactivated, if there is a safety risk. Overall structure is shown in the picture below.

 ![disarming.jpg](../pics/lynx_safety_disarming.jpg)

There are several sources, considered as potential safety risk initiators. These are divided into categories stated below

**Faulty component:**
- Driver in error
- I/O block in error
- BMS in error
- Throttle signal is `Not a Number`
- Braking signal is `Not a Number`

> `Not a Number` is evaluated in the particular ASC block - the signal is outside absmin and absmax region 

**Danger state:**
- BMS is in charging mode
- Safety switch signal
- Active brake and accelerate signal at one moment
- System in the safety map (map0) 


**Danger transition:**
- Change to the reverse map

If any of these inputs is valid, then the disarming block can reacts in several ways:
- **Temporary disarming -** The most common type of reaction, acceleration (adn cruise) is disabled until the source of its activation is gone (e.g. driver error or BMS in charging mode)
- **Permanent disarming -** Special case, acceleration is disabled until the activation criteria is gone and controller is restarted
- **Transition disarming -** Event driven reaction, the acceleration is disabled for the transition time period (e.g. transition to the reverse map) 

Reaction of majority of these inputs is hardcoded, but there is also option for parametrization of disarming block by parameters `safetyopts` and `drvopts`.

Status of disarming block can be checked in state variable `armed_state`

### Redundant throttle signal

The system offers second analogue throttle input for ensuring higher safety of the Ride-by-Wire accelerator. This feature is comparing both throttle signals and if their difference is above the level set by value in parameter `dual_err` then the acceleration is disabled. Structure is shown in the picture below

>It is possible also to use just a digital swith for checking zero position of the throttle 

 ![safetythrottle.jpg](../pics/lynx_safety_throttle.jpg)

## Cruise

Cruise function is currently disabled for all Lynx builds

Block structure is shown in the picutre below

 ![cruise.jpg](../pics/lynx_cruise.jpg)
### Assisted walk function

It is a special mode of cruise. This feature can be helpful for pushing a bike up to a hill. Walk is activated by pressing and holding of the mapped input button and is working unitl the button is relased. Speed level of the assist function can be set by parameter `walklvl`

>This function can be activated if in_cruise is mapped to a valid physical input and speed of the motor (at the time of activation) is lower then `walklvl`

>It is possible to use the same physical input for in_cruise (long press) and in_mapswitch (short press)

## Other functions

### Vehicle speed calculation

Lynx offers two possible methods for vehicle speed evaluation.

- **Motor speed:** Information from the driver is used for speed determining. It does not need to install any external sensor. But this option requires correctly set parameters of the drive train (motor pole pairs - `driver/motor/pp`, gear ratio - `gearthr` and driver wheel size - `odothr`). Also this solution is not working when a motor with freewheel clutch is used

- **External speed sensor:** Pulses from the external speed sensor are used for speed evaluation. All the necessary settings is made in Tachospeed block (output is wheel rotation speed). Vehicle speed is compute using wheel size information (`odothr`) then

>External speed sensor is activated if `in_speed` is mapped to a valid physical input

>If Bafang display is used, then the system is sending only information about one wheel revolution. Appropriate settings is made in the dispaly then

Some advance settings of the speed calucation can be done by parameter `speedopts`

### Timers

Lynx offers two settable timers

- **Warning timer:** This timer can be used for sound signalization when the system is armed but idling (to aware user that it is energized). Warning timer can be set by paramater `warntim`

- **Shutdown timer:** This timer can be used for automatic powering off when the system is inactive (idling). Shut down timer can be set by parameter `shdntim`

### Sound signalization

There is a possibility to indicate several states of the system by acoustic signalization using the motor as a speaker. Sound signalization can be us for these states:
- Motor is armed
- Motor is disarmed
- System is powered on
- System is powered off
- System is locked 
- Battery SOC
- Throttle malfunction
- Map change

The setting of this feature can be done by `beepopts` parameter

### Master-Slave operation

The system offers cooperation of up to 4 devices (1 master and 3 slaves) interconnected and communication via CANbus. The benefits are that all the control inputs are connected to the master only and it is giving commands to all slaves. Activation and setting of this function is done by parameter `syncmode` in all the devices on the bus 

[More info here](./guide/multi_controller_setup.md)
>All devices on the CANbus needs to have unique addresses (master has always address - 0)

### Displays

Lynx supports several ways of visual signalization of its states. Parameter `dispopts` defines what device is used for the signalization. Currently supported devices are listed below

- **Programmable LED (WS):** The simplest indication by a programmable LED connected to the UART interface. LED properties can be set by parameters `ledbright` and `ledopts`
- **Screens using Bafang's UART protocol:** This solution can be used for indication of ride data (speed, user map, SOC) as well as external digital inputs used as map switch button and two programmable buttons

### Lights

There is an option to control external lights using inbuilt digital output (if the device is equipped with it). The lights are controlled by the state of the digital `in_light` input. Advanced settings of lights can be done by parameter `lightopts` 

> The possibility to connect lights depends on the device HW configuration

> This function is activated if `in_light` is mapped to a valid physical input
