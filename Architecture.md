# Hardware architecture #

You can think of it as USRP N2x0 with integrated two LimeMicro LMS6002D transceivers, GPS locked 13MHz clock and other bonuses. More detailed description is to be written.

## FPGA buses and peripherals ##

| 2x LimeMicro LMS6002D | 2x SPI<br />2x 12-bit parallel data Rx<br />2x 12-bit parallel data Tx |
|:----------------------|:-----------------------------------------------------------------------|
| TCXO DAC              | SPI                                                                    |
| Flash                 | SPI                                                                    |
| GPS                   | UART<br />1pps                                                         |
| Debug UART            | UART / Mini-USB                                                        |
| SRAM                  | 21-bit parallel address<br />36-bit parallel data<br />9-bit parallel control |
| 1 GbE PHY             | 8-bit parallel Tx<br />8-bit parallel Rx<br />parallel control<br />2x GPIO (LEDs) |
| Temperature sensor    | 2x GPIO (FAN\_ON and OVERHEAT signals)                                 |
| Buttons               | 1x GPIO (SAFE)<br />FPGA RESET                                         |
| LEDs                  | 5x GPIO<br />FPGA DONE                                                 |
| Debug FPGA connector  | 32-bit parallel bus                                                    |
| Mezzanine connector   | 7x GPIO                                                                |

## Other things to test ##

  * FPGA JTAG
  * Flash JTAG
  * FAN1 and FAN2 power
  * Clock connector and switches
  * Clock/GPS status LEDs

## Clocking domains for Tx chain ##

![http://wiki.umtrx.googlecode.com/hg/images/documentation/fpga-tx-clocking.jpg](http://wiki.umtrx.googlecode.com/hg/images/documentation/fpga-tx-clocking.jpg)