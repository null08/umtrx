# Introduction #

Power consumption of the transceiver can reach 20 W or even a bit more and it must be dissipated stably in time to get stably good specs as it was right after calibration. In ideal you need either thermal stabilization or repeating calibration after every significant temperature change. Anyway should be used some cooling solution to dissipate all heat.

# Details #

## Temperature sensing ##

In the UmTRXv2.1 there are thermal sensors U38 (TMP102) to measure board temperature in the most hot place. For UmTRXv2 should be used external thermal sensors with SPI or I2C interface and connected to X12 (AUX RF). This will help to know when re-calibration required or to shifting look-up table for new constants to compensate drift of parameters.
Also there is U35 (MAX6665ASA45) which will warn FPGA when temperature reached +45°C ("FAN\_ON" FPGA\_pin\_AB14), when temperature over +60°C ("OVERHEAT" FPGA\_pin\_AB13) and SHUTDOWN transceiver if temperature over  +75°C.

## Cooling ##

There are two easy ways to dissipate heat power.

### FAN-less solution ###

Firstly is FAN-less solution with heatsink, which is mostly for outdoor usage in waterproof housing. Should be used thermal-gap-filler with 3~5 mm thickness to make good thermal conduction from UmTRX to heatsink.

### FAN solution ###

Second solution with FAN's, which is much easy to implement, but it is mostly for indoor usage. It is possible to use one or even two FAN's connected to the board. There are two connectors on the board: X17 (FAN1) and X18 (FAN2). FAN1 constantly powered. FAN2 will automaticaly switched On/Off by U35 (MAX6665ASA45) when temperature around  45°C. Maximum DC current of  FAN2 must be less then 400mA.
In UmTRXv2 both connectors (together) are powered either from input voltage  8.. 28V (by default) or from DC/DC converter  6V (just re-solder 0603 jumper near connectors for this).
In  the UmTRXv2.1 both connectors (separately) are powered either  from DC/DC converter  6V (by default) or from input voltage  8.. 28V (just re-solder 0603 jumpers near connectors for this).