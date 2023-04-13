---
title: "Hybrid throttle"
weight: 21
---
# Hybrid throttle - EXPERIMENTAL FEATURE

> This is EXPERIMENTAL FEATURE. It may not work properly. Use with caution.  
> 



Idea: At low speed throttle controll voltage (speed). At high speed throttle controll current (torque).


## Enable this feature

Set parameter `/acc/drvmode` to value 40.  
This will enable Hybrid throttle feature.  After that tune next parameters to get good feel from the throttle.


## Parameters

Located: **`/acc/acc_opts/`**

| name | description|
| --- | --- |
| `thr_curr_offset` |     At voltage mode, you can scale also current. <br> 0 - current is scaled with voltage <br> 1 - current is always na 100% and only voltage is scaled by throttle  |
| `CUR_enter_margin` |  If drv_rel_voltage is higher than this, throttle enters current mode |
| `VLT_enter_margin` |  If drv_rel_voltage is lower than this, throttle enters voltage mode |


## States

Located: **`/acc/acc_opts/`**

| name | description|
| --- | --- |
| `drv_rel_voltage` |     Relative voltage = Motor relative speed 0 - 1 |
| `drv_rel_current` |     Relative current = Motor relative current 0 - 1 <br> 1 - phase current = iref|
