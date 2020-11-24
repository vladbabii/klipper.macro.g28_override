# Homing (G28) override for Klipper [v.2]

This macro overrides klipper ( https://github.com/KevinOConnor/klipper ) homing process to make a better customisation and

## How to install 
1. download file through any method (file download, git clone, etc) to the computer running klipper 
2. add to configuration ( https://github.com/KevinOConnor/klipper/blob/master/docs/Config_Reference.md#include )
```
[include path/to/folder/macro_g28_override.cfg]
```
3. also add to configuration if respond is not already configured ( https://github.com/KevinOConnor/klipper/blob/master/docs/Config_Reference.md#respond )
```
[respond]
```
4. configure homing as described below

## Configuration
You need to add some configuration for these macros to be useful,  for example this one hax X as dependency of y and x and y as dependency of z, and the order of homing is y then x then y
```
[gcode_macro HOMING_OVERRIDE_CONFIG]
variable_order: "y,x,z"
variable_dowith_x: "y"
variable_dowith_y: "x"
variable_dowith_z: "x,y"
variable_start_zhop: 20
gcode:
  RESPOND PREFIX="info" MSG="Homing config"
```
Read below for description of settings

## Features

### Keep track of actual homing status and can allow homing of each axis only when needed

When zero value is specified axis will not be homed if already "real" homed
  G28 X0 - will home only if needed
  G28 X - will always home X, regardless of status

### Home Ordering
By editing adding the following configuration in your printer config file you can change the default homing order
```
[gcode_macro HOMING_OVERRIDE_CONFIG]
variable_order: "x,y,z"
gcode:
  RESPOND PREFIX="info" MSG="Homing config"
```
Just edit the "x,y,z" and put any order you want in it. 
Make sure all axis you want to home are set there, since if you skip one it will not be homed. 
Use lowercase letters for all axies.

### Homing Dependencies
Sometimes homing an axis requires another one to be also homed, for example you want to home Y before X every time. In this case G28 X should home Y then X. This is resolved by setting homing order (see above) to "y,x,z" then set Y as a dependency of x by adding this to your config file
```
[gcode_macro HOMING_OVERRIDE_CONFIG]
variable_dowith_x: "y"
gcode:
  RESPOND PREFIX="info" MSG="Homing config"
```

## Homing Z-hop start
If you want to start homing by lifting the printhead (relative to bed), you can use this setting 
```
[gcode_macro HOMING_OVERRIDE_CONFIG]
variable_start_zhop: 20
gcode:
  RESPOND PREFIX="info" MSG="Homing config"
```

## Overriding individual axis homing
For each axis you can create a macro named "HOMING_OVERRIDE_<axis_name>" like below. Each gcode must contain *G990028 X0* gcode command - replace X with the axis you're overriding
```
[gcode_macro HOMING_OVERRIDE_X]
gcode:
  RESPOND PREFIX="info" MSG="Homing > X"
  G90
  G990028 X0
  G91
  G0 X5 F2000
  G90
  
[gcode_macro HOMING_OVERRIDE_Y]
gcode:
  RESPOND PREFIX="info" MSG="Home > Y"
  G90
  G990028 Y0
  G91
  G0 Y-5 F2000
  G90
  
[gcode_macro HOMING_OVERRIDE_Z]
gcode:
  RESPOND PREFIX="info" MSG="Homing > Z"
  G90
  G0 X100 Y100
  G990028 Z0
  G91
  G0 Z20
  G90
```
You can override only the axis you need.

