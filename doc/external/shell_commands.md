---
title: "Commands"
weight: 13
---

# YOS commands

These commands are for LYNX application. Other command are here: [Driver commands](./../../_ESC/doc/external/driver/shell_commands.md), [YOS commands](./../../lib/_YOS/doc/external/shell_commands.md)


### `pmp [map no]`

Power MaP - Change power map from terminal. 


### `trip_reset`

Reset trip counter.

### `playsoc [soc 0-1]`

You can enable vehicle starting sound, that play melody according battery SOC. To test this sound you can use this command. Without parameters, it will play actual SOC. With parameter: `soc 0.5` it will play SOC sound for 50%.
