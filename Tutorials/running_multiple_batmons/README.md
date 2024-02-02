# Running Multiple Batmons on One Bus
## Physical Layer
The Opto-Isolation of the signals is **always** recommended to reduce noise and improve reliability on the bus, and is **absolutely necessary on the higher battery** when the batteries are in series. 

![Opto-iso I2C block diagram](http://batmonfiles.rotoye.com/userguide/Opto-iso%20connection.jpg)

When using two Batmons on the same bus, an I2C splitter, like the one shown here, is useful:  
![I2C signal splitter](http://batmonfiles.rotoye.com/userguide/splitter.png)

## Batmon Firmware
When using two Batmons on the same I2C bus, they cannot have the same I2C slave adress. This needs to be changed in the Batmon Firmware, in:
 
Batmon_firm/batmon_v3/battery_config.h

`BATMON_SMBUS_ADDRESS` Must be set to a different value for each Batmon that is to be used on the same bus. 

If Batmons are being used on two seperate bus interfaces, no changes are required in firmware. 

## Interfacing 

#### PX4

As of PX4 v1.11, multiple smart batteries with different SMBus addresses can be used on the same bus.

Clone or download [Rotoye's fork of PX4:](https://github.com/rotoye/px4_firmware_batmon)
    
Checkout the batmon_v2.02_px4_v1.11 branch
    
Build and upload the firmware according to [PX4 documentation instructions](https://dev.px4.io/master/en/setup/building_px4.html)
    
In ground control software of choice, set the BATx_SOURCE parameters to "External", and set SENS_EN_BAT to true
    
Start the batt_smbus [script](https://dev.px4.io/master/en/middleware/modules_driver.html) on NuttShell

For example, running two BatMons on the same bus:

`batt_smbus start -X -b 1 -a 11` * External bus 1, address 0x0b  
`batt_smbus start -X -b 1 -a 12` * External bus 1, address 0x0c

####  Ardupilot

As of ArduPilot 4.2.0, multiple smart batteries with different SMBus addresses can be used on the same bus.

For example, using params   
``BATT_I2C_ADDR = 11``
``BATT_I2C_BUS = 2``
``BATT_MONITOR = 19: Rotoye``

``BATT2_I2C_ADDR = 11``
``BATT2_I2C_BUS = 3``
``BATT2_MONITOR = 19: Rotoye``

``BATT3_I2C_ADDR = 12``
``BATT3_I2C_BUS = 3``
``BATT3_MONITOR = 19: Rotoye``

Would allow for using three Batmons on seperate external I2C busses

