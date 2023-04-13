---
title: "Smooth acceleration"
weight: 22
---

# Smooth acceleration

Smooth transition when going from standby mode to acceleration or brake.

![smooth acc](/lynx_smooth_acc.jpg) 

This feature implements Low Pass Filter to the command. But from LPF in /acc/csc folder, where the lpf is applied all the time to the throttle signal, here is applied only if you are going from standstill to ride.  
This is usefull, if you have gearbox. 

## Parameters

| name | value | description|
| --- | --- | -- |
| smooth_lpf | 0-1 ( [lpf](./../../lib/_LIB/doc/external/lpf.md)) |      1 - disable this functionality <br> Lower value = more delay |
