---
title: "Kickback scooter helper"
weight: 20
---

# Kickback scooter helper

This feature will extend your travel distance on kickback scooter.  

Any brake input will disable this. So you can not use it with combined brake.  

![graph](./../../pics/lynx_kickback_helper.jpg)

> ## Safety notes
> Always use brake input with this feature!  
> Wrong combination of parameters can cause uncontrolled motor spin!



## Parameters

All parameters are in folder **`/kickback`**

| Name | value | description |
| --- | --- | --- |
| `maps` |  Bitwise parameter | Bit position represent map, where kickback mode is active. 1 = map0, 2 = map1, 4 = map2, 8 = map3, ... |
| `ramp_dn` | | How fast your vehicle slow down in helper mode. |
| `min_rel_vlt` | 0 - 1 | Minimal relative speed to enable kickback mode. |
| `trqlvl` | 0 - 1 | Torque for helping in this mode.|
