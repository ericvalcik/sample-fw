---
title: "Maps"
weight: 8
---

# Power maps



Parameters in `/maps/mapX`
| Name | Value | Description |
| --- | --- | --- |
| `kph` | [kph] | Speed limitation |
| `pwr` | [W] | Power limitation |
| `trqlvl` | [0-1] | Global torque multiplier (imult)
| `acclvl` | [0-1] | Throttle signal multiplier 
| `paslvl` | [0-1] | 
| `pastrq` | [0-1] |
| `comlvl` | [0-1] | Combined brake signal multiplier
| `sbrakelvl` |[0-1] | Static brake multiplier
| `options` | Bitwise parameter, description below |




| `options` value | Description | 
| --- | --- |
| 1 | Disable static brake |
| 2 | Disable dynamic brake |
| 4 | Disable cruise |
