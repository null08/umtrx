# Introduction #

Power consumption of transceiver has relevance for a particular application.
For example it is very important for solar or diesel powered BTS.
Power consumption of UmTRXv2 transceiver in dual channel mode is around 14..16 W and depends of functionality, Tx output power and supply voltage.

# Details #

In the tables below you can find power consumption in depending of functionality, Pout and supply voltage.

### Power consumption dependency on functionality of the UmTRXv2.1 ###

**Measurement conditions:** Vcc=12.0V (power calculated from the DC current of Vcc supply), Ta=25 deg C, passive heatsink, active GPS antenna, Fout=942/942.2MHz, Pout=20/20 dBm modulated by tx\_samples\_from\_file.exe with args: --rate 1083333 --gain 15, receiving by rx\_samples\_to\_file.exe with args: --rate 1083333 --gain 15, mezzanine: UmSELv1-GSM900.

| <br />Function| **Power delta, W** | **+1 LMS,**<br />Power delta, W| **+2<sup>nd</sup> LMS,**<br />Power delta, W| <br />Total, W|
|:--------------|:-------------------|:-------------------------------|:--------------------------------------------|:--------------|
| Power apply   | 4.08               | -                              | -                                           | 4.08          |
| GBEth connect | 1.44               | -                              | -                                           | 5.52          |
| LMS Rx to file | -                  | 2.40                           | 2.40                                        | 10.32         |
| LMS Tx from file | -                  | 2.88                           | 2.88                                        | 16.08         |
| Mezzanine On  | 4.08               | -                              | -                                           | 20.16         |

### Power consumption dependency on functionality of the UmTRXv2 ###

**Measurement conditions:** Vcc=12.0V, Ta=25 deg C, passive heatsink, active GPS antenna, Fout=942/942.2MHz, Pout=20/20 dBm continuous ideal GMSK, TXVGA1=-10, TXVGA2=20, no mezzanines.

| <br />Function| **Power delta, W** | **1 LMS,**<br />Power delta, W| **+2<sup>nd</sup> LMS,**<br />Power delta, W| <br />Total, W|
|:--------------|:-------------------|:------------------------------|:--------------------------------------------|:--------------|
| Power apply   | 3.96               | -                             | -                                           | 3.96          |
| GBEth connect | 1.44               | -                             | -                                           | 5.40          |
| LMS Rx Enable | -                  | 2.16                          | 2.16                                        | 9.72          |
| LMS Tx Enable | -                  | 1.56                          | 1.56                                        | 12.84         |
| LMS PA On     | -                  | 0.84                          | 0.84                                        | 14.52         |

### Power consumption dependency on Pout (TXGAIN) of the UmTRXv2.1 ###

**Measurement conditions:** Vcc=12.0V, Ta=25 deg C, passive heatsink, active GPS antenna, modulated by tx\_samples\_from\_file.exe with args: --rate 1083333, receiving off, no mezzanines.

**Note:** Matching [issue 41](https://code.google.com/p/umtrx/issues/detail?id=41) of the original UmTRXv2 is fixed in UmTRXv2.1 on the PCB layout and BOM.

| **--gain** | **Pout GSM900, dBm**  | **Pout GSM1800, dBm** | **P, W** |
|:-----------|:----------------------|:----------------------|:---------|
| 0          | 7                     | 3                     | 9.6      |
| 3          | 10                    | 6.5                   | 10.08    |
| 6          | 13                    | 9.7                   | 10.32    |
| 9          | 16                    | 12.8                  | 10.8     |
| 12         | 18.1                  | 16                    | 11.28    |
| 15         | 19.7                  | 18.3                  | 11.28    |
| 18         | 20.2                  | 19.2                  | 11.28    |
| 21         | 19                    | 19.5                  | 11.04    |

**Note:** Spectrum becomes quite dirty at the gain 18 and above.

### Power consumption dependency on Pout (TXVGA2) of the UmTRXv2 ###

**Measurement conditions:** Vcc=12.0V, Ta=25 deg C, passive heatsink, active GPS antenna, continuous ideal GMSK, TXVGA1=-10, no mezzanines.

**Note:** Big difference between GSM900 and GSM1800 bands in this table is because of the matching issue in the original UmTRXv2. This is fixed now (see [issue 41](https://code.google.com/p/umtrx/issues/detail?id=41)), new measurements are pending.

| **TXVGA2** | **Pout GSM900, dBm**  | **Pout GSM1800, dBm** | **P, W** |
|:-----------|:----------------------|:----------------------|:---------|
| 1          | 2.4                   | ~~-12.8~~ (see above) | 13.2     |
| 5          | 6                     | ~~-9~~ (see above)    | 13.38    |
| 10         | 11                    | ~~-4~~ (see above)    | 13.44    |
| 15         | 16                    | ~~0.9~~ (see above)   | 13.92    |
| 20         | 19.7                  | ~~6~~ (see above)     | 14.52    |
| 25         | 21.5                  | ~~11~~ (see above)    | 15.72    |

### Power consumption dependency on supply voltage of the UmTRXv2 ###

**Measurement conditions:** Ta=25 deg C, passive heatsink, active GPS antenna, Fout=942/942.2MHz, Pout=20/20 dBm continuous ideal GMSK, TXVGA1=-10, TXVGA2=20, no mezzanines.

| **Supply voltage** | **Current, A** | **Power, W** |
|:-------------------|:---------------|:-------------|
| 8.0                | 1.79           | 14.32        |
| 9.0                | 1.59           | 14.31        |
| 12.0               | 1.21           | 14.52        |
| 15.0               | 0.97           | 14.55        |
| 24.0               | 0.63           | 15.12        |
| 27.0               | 0.57           | 15.39        |
| 28.0               | 0.55           | 15.40        |