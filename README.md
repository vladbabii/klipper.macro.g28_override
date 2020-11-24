# Homing (G28) override for Klipper

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

## Features

### Keep track of actual homing status and can allow homing of each axis only when needed

When zero value is specified axis will not be homed if already "real" homed
  G28 X0 - will home only if needed
  G28 X - will always home X, regardless of status
