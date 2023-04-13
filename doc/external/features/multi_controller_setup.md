---
title: "Multi controller setup"
weight: 50
---

# Multi controller setup

Purpose of this guide is to setup LYNX application with 2 or more controllers. One controller is a Master. To the Master you will wire all the inputs. Other controllers are connected just over CAN.

Example usage:
- Bike with front and rear hub motor
- Electric cart with 2 motors, one for each rear wheel
- Quad bike with 4 individual motors

## Implementation notes

- If Master is in error, whole vehicle is stopped
- If Slave is in error, only one motor is stopped (in freewheeling mode)
- What is the Master sending to the Slave:
    - Driver mode, driver cmd and imult
    - LYNX selected map
- Only Master is sending LYNX [CAN messages](./../messages.md)
- Driver limiter are working independently
    

## Prerequisites

- Completelly setup Master controller (controller with address 0)
    - Motor is identified
    - Inputs are wired 
    - The vehicle can be driven with only the Master without problem
- Prepare Slave controllers    
    - Setup same CAN speed
    - Make sure, that you have two 120ohm CAN terminators on the bus
    - Setup different address for each Slave (starting from address 1)
    - Identify motors on Slave controllers
    - Setup driver in Slave controllers
        - limiters, current (can be different from master)

## LYNX preparation

- In Master set parameter:
    - `/misc/syncmode` -> `1`    
- In all Slaves set parameters
    - `/misc/syncmode` -> `2` 
    - Other parameters leave in default
- Copy these parameters from Master to Slave (Also remember, when you change these parameters, change them in all controllers)
    - `maps/restmapcnt`    
    - `maps/mapchlpf`
    - `maps/mapcnt`


## Maps tuning

List of parameters, that can be individually tuned in Slave and Master:

- `maps/kphlimit`
- `maps/pwrlimit`
- In every map:
    - `maps/mapX/kph`
    - `maps/mapX/pwr`
- Other parameters in maps are not used in Slave  


> Examples:  
> **I want same power and speed from Master and Slave**  
> Just copy all above listed parameters from Master to Slave  
> 
> **I want front motor with less power**  
> Set different values in `pwr` parameter in maps.  
>
> **I want different torque in front motor**  
> Change `/driver/iref`
