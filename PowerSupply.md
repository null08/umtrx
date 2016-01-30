**Table of Contents**


# Power supply connector #

<img src='http://media.digikey.com/photos/Molex/39-30-1022.JPG' width='150' />
<img src='http://media.digikey.com/photos/Molex/39-01-3022.JPG' width='150' />

**Molex Mini-Fit 39-30-3022 (socket), 39-01-3022 (plug)**

  * Datasheet - [pdf](http://www.molex.com/pdm_docs/sd/039013022_sd.pdf)
  * Digi-Key - [socket](http://search.digikey.com/us/en/products/39-30-1022/WM21363-ND/930332), [plug part 1](http://search.digikey.com/us/en/products/39-01-3022/WM1021-ND/1118564), [plug part 2](http://search.digikey.com/us/en/products/39-00-0038/WM2501CT-ND/467978)

# Power supply requirements #

**UmTRXv1** can be power with a **stabilized 7.5V..15V DC**.

**UmTRXv2** can be power with a **stabilized 8V..28V DC**.

  * A standard 12V battery or solar panel supply should work.
  * Supply voltage above 15V is not recommended for UmTRXv1. Primary DC supply circuit contain alluminium capacitors 470uF/25V which work in dynamic mode because of DC-DC converters. It's recommended to keep supply voltage below 60% of capacitors nominal. 15V=0.6\*25V
  * There are no any specific requirements for power supply quality except of possibility of load for 15 W for a long time. Note, that adding daughter boards such as UmSEL can increase power consumption up to 20 W.

# Power-over-Ethernet #

For indoor pico-BTS installations [PoE](https://en.wikipedia.org/wiki/Power_over_Ethernet) is almost a must these days. For UmTRX we need about 20W power and thus we have to use PoE+ aka IEEE 802.3at-2009 standard.

Right now we could use an external PoE splitter. Downside of a separate splitter is that it is not really cheap. Examples of PoE+ splitters:
  * http://www.planet.com.tw/en/product/product_spec.php?id=39714 (~$150 in Russia)

Whether we should integrate this into UmTRX or not is a good discussion. We may consider this for one of the later revisions.

# How to measure power consumption #

You can control current with the help of filters TI160808U601 and TI201209U121 which exist in all important power rails. They have the internal resistance of approx 0.3 and 0.1 Ohms respectively.

The accuracy of those measurements may not be as high as with resistors, but sufficient for a rough estimate. You could replace those filters with 1% resistor if you need higher accuracy and/or dynamic values.


# Power consumption measurements #

  * _Prototype 1, 03.01.2012:_ I power it from 8v and it takes ~ 0.5 A of static current (no fw loaded).
  * _Prototype 1, 14.01.2012:_ With loaded FPGA bistream (SRAM and Ethernet are enabled) in idle mode is approximately 6W:
    * Current at 6.5V was 0.89A (5.785W)
    * Current at 12V was 0.51A (6.12W)
<a href='Hidden comment: 
* _Prototype 1, 15.01.2012:_ I"ve just made some measurements :
```
L10-1    1.2 mv    0.3R    LMS1 1.8v Analog
L7-1     4.2 mv    0.3R    LMS1 3.3v
L9-1     1.3 mv    0.3R    LMS1 1.8v Digital
L8-1     1.8 mv    0.3R    LMS1 2.5v
L10-1    1.2 mv    0.3R    LMS1 1.8v Analog
L7-1     4.1 mv    0.3R    LMS1 3.3v
L9-1     1.3 mv    0.3R    LMS1 1.8v Digital
L8-1     1.9 mv    0.3R    LMS1 2.5v
L1      10.2 mv    0.3R    5V2 rail
L2       2.8 mv    0.3R    GPS
L34     10.2 mv    0.08R   6.5v
L40      0.4 mv    0.08R   3.3v FPGA
L41      1.0 mv    0.08R   2.5v (FPGA+Clock+RF)
L42      1.0 mv    0.08R   2.5v ETH
L43      0.3 mv    0.08R   2.5v RAM
L3       5.7 mv    0.3R    2.5v clock
L47     13.2 mv    0.3R    5v RF
L48      0.1 mv    0.3R    2.5v RF

Total: 917mW (doesn"t include 1.2V FPGA core).
This is the power consumption of the system without the power conveters loss.
```
So you can see that from the main rails, the 6.5V one (which is the main smps rail for all subsequent linear supply) is the biggest current hog. And all that power goes mainly to the LMS and to the RF LNA. And in a smaller part to the GPS and oscillator. (this is from a dummy firmware that just blinks leds, so FPGA loaded, but not doing much)

My multimeter is "only" 10.000 count and isn"t good enough to measure the exact series resistance of each of these filters to get absolute values. You"d need a proper LCR to measure them I guess, but I don"t have any.
'></a>
  * _Prototype 1, 18.01.2012:_ measurements for idle mode ([xls attached here](http://code.google.com/p/umtrx/downloads/detail?name=Power_consumption_idle_18.01.2012.xls&can=2&q=))
    * Total consumption, measured at DC supply is 3.8W @ 8V.
    * Each LMS consumes about 1 W in idle mode.
    * For approx tests I suggest to use the calculated values of 0.025 Ohm for TI2012 and 0.14 Ohms for TI1608.
    * The efficiency DC-DC converters is more then 85%.
  * _UmTRXv2, 20.10.2012:_ [UmTRXv2 Power measurements](UmTRXv2_Power.md)