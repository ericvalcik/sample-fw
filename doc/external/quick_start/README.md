---
title: "Quick start with LYNX"
weight: 1
---

# Quick start with LYNX

This guide will guide you through the basic settings of firmware LYNX. After this guide you will have setup LYNX with throttle and map switch.

LYNX firmware is shared across all siliXcon controllers.

## Safety precaution 

- Read this guide and be sure, that you understand all the
- Lift your vehicle from the ground
- Secure the vehicle against any movement 
    - The motor will spin later
- 

## Prerequisites

- You have siliXcon controller with LYNX firmware
    - It can be VECTOR_LYNX_xxxx or BLDC_LYNX_xxxx
- You know [How to change parameters in the controller using emGUI](https://silixcon2.atlassian.net/servicedesk/customer/portal/3/article/295769)
- You are starting with the default configuration
    - [How to restore default parameters](https://silixcon2.atlassian.net/servicedesk/customer/portal/3/article/295710?src=1314780726)




## Wiring

Check hardware documentation, for the pinout for your controller.
[Hardware documentation for controllers](https://doc.silixcon.cloud/hw/doc/latest/controller/)

Connect the motor and motor sensor to the controller
- Connect throttle (potentiometer) to GPIO0
- Connect powering switch
- Connect battery
- Identify the motor

[All these steps are explained more here](https://silixcon2.atlassian.net/servicedesk/customer/portal/3/article/295876?src=1265547795)



### Connect map button:

The map button will allow you to change the power map. In each map, you can set max speed, torq or power.

- Connect the map button to GPIO1 and IOGND

> Note:  
> If you connect button or throttle to different input it is ok. Correct input can be set later by parameter.


**[Next step: Setup throttle parameters](./throttle.md)**
