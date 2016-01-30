**Table of contents**


## Calibrating LMS6002D ##

The process is detailed at the [LMS6002D Calibration page](LMS6002DCalibration.md).

Found values could be stored to EEPROM to be used automatically after reboot. See below how to store to EEPROM.

## Calibrating TCXO ##

Use `./host/utils/umtrx_vcxo.py` to set TCXO DAC value in realtime to find the best calibration value. The simplest way to find the right value if you don't have a stable reference to compare with, is to use [Kalibrate-UHD](https://github.com/ttsou/kalibrate-uhd) to calculate UmTRX frequency offset relative to a nearby GSM base station.

Found value could be stored to EEPROM to be used automatically after reboot. See below how to store to EEPROM.

## Storing values to EEPROM ##

Right now we have the following values which could be stored in EEPROM:
  * tx-vga1-dc-i - DC offset calibration value for LMS6002D Tx I channel
  * tx-vga1-dc-q - DC offset calibration value for LMS6002D Tx I channel
  * tcxo-dac - TCXO calibration value

Now, when the framework for UmTRX EEPROM is in place, we could store more values there as we see the need. E.g. we may want to store Rx DC offset calibration values or power limits of this exact units to be used by OpenBTS.

The only limitation here is the size of the EEPROM. EEPROM is only 256 bytes large and some of them are already used, so we have about 200 bytes to use. With the strong dependency of calibration values on temperature and frequency, we might need a two-dimensional array for each value. In this case we may want to move those calibration values to flash instead of EEPROM. This would need some more work and thus is in our wish-list for now.

### Writing values ###

You could write values mentioned above to EEPROM as any other EEPROM value:
```
  ./host/build/utils/usrp_burn_mb_eeprom --key <name> --val <dec_val>
  (from the root of your UHD source tree)
```

### Reading values ###

You could read individual values from EEPROM as any other EEPROM value:
```
  ./host/build/utils/usrp_burn_mb_eeprom --key <name>
  (from the root of your UHD source tree)
```

Or you could use `uhd_usrp_probe` to see them all at once at the top-level section:
```
  _____________________________________________________
 /
|       Device: UmTRX Device
|     _____________________________________________________
|    /
|   |       Mboard: UMTRX-REV0
|   |   hardware: 64000
|   |   mac-addr: 00:50:c2:85:3f:ff
|   |   ip-addr: 192.168.10.4
|   |   gpsdo: none
|   |   serial: 1
|   |   tx-vga1-dc-i: 114
|   |   tx-vga1-dc-q: 138
|   |   tcxo-dac: 2022
|   |   
|   |   Time sources: none, external, _external_, mimo
|   |   Clock sources: internal, external, mimo
|   |   Sensors: ref_locked
|   |     _____________________________________________________
|   |    /
|   |   |       RX DSP: 0
|   |   |   Freq range: -6.500 to 6.500 Mhz
|   |     _____________________________________________________
|   |    /
|   |   |       RX DSP: 1
|   |   |   Freq range: -6.500 to 6.500 Mhz
|   |     _____________________________________________________
|   |    /
|   |   |       RX Dboard: A
|   |   |   ID: LMS RX (0xfa09)
|   |   |   Serial: 1
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       RX Subdev: 0
|   |   |   |   Name: LMS RX (0xfa09) - 0
|   |   |   |   Antennas: RX0, RX1, RX2, RX3, CAL
|   |   |   |   Sensors: 
|   |   |   |   Freq range: 232.500 to 3720.000 Mhz
|   |   |   |   Gain range VGA2: 0.0 to 30.0 step 3.0 dB
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       RX Codec: A
|   |   |   |   Name: LMS_RX
|   |   |   |   Gain Elements: None
|   |     _____________________________________________________
|   |    /
|   |   |       TX DSP: 0
|   |   |   Freq range: -32.500 to 32.500 Mhz
|   |     _____________________________________________________
|   |    /
|   |   |       TX Dboard: A
|   |   |   ID: LMS TX (0xfa07)
|   |   |   Serial: 1
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       TX Subdev: 0
|   |   |   |   Name: LMS TX (0xfa07) - 0
|   |   |   |   Antennas: TX0, TX1, TX2, CAL
|   |   |   |   Sensors: 
|   |   |   |   Freq range: 232.500 to 3720.000 Mhz
|   |   |   |   Gain range VGA: -35.0 to 21.0 step 1.0 dB
|   |   |   |   Connection Type: IQ
|   |   |   |   Uses LO offset: No
|   |   |     _____________________________________________________
|   |   |    /
|   |   |   |       TX Codec: A
|   |   |   |   Name: LMS_TX
|   |   |   |   Gain Elements: None
```