# General #

_We're filling this as we go, so it's not complete yet. More to be added later._

The default configuration of UmTRX is ideal for GSM:
| Reference clock frequency | 26 MHz |
|:--------------------------|:-------|
| Reference clock stability | 0.1 ppm = 100 ppb |

# UmTRX Stacking #

UmTRX have an option for stacking, but it is only to share clock source and/or GPS source. I.e. you still have to connect each UmTRX to the Ethernet. As long as connecting to an Ethernet switch works fine, so we don't see this as an issue.

Later we may consider implementing true stacking when stacked units share Ethernet port, like with USRPs. This might be a useful endeavor, but we don't want to dive into this until we have everything else settled.

# UmTRX LEDs #

UmTRX has tree 3-LED lines and three separate LEDs:

LED1 line:
| **LED name** | **Description** |
|:-------------|:----------------|
| **LED A**    | **Tx1 (Channel A) status:**<br>OFF — The module is not transmitting radio waves, i.e. data is not being sent to DAC.<br>GREEN — The module is transmitting radio waves, i.e. data is being sent to DAC.<br>
<tr><td> <b>LED C</b> </td><td> <b>Rx1 (Channel A) status:</b><br>OFF — The module is not receiving radio waves, i.e. data is not being received from ADC.<br>GREEN — The module is receiving radio waves, i.e. data is not being received from ADC.</td></tr>
<tr><td> <b>LED E</b> </td><td> <b>Tx2 (Channel B) status:</b><br>OFF — The module is not transmitting radio waves, i.e. data is not being sent to DAC.<br>GREEN — The module is transmitting radio waves, i.e. data is being sent to DAC.</td></tr></tbody></table>

LED2 line:<br>
<table><thead><th> <b>LED name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <b>LED B</b>    </td><td> <b>Rx2 (Channel B) status:</b><br>OFF — The module is not receiving radio waves, i.e. data is not being received from ADC.<br>GREEN — The module is receiving radio waves, i.e. data is being received from ADC.</td></tr>
<tr><td> <b>LED D</b>    </td><td> <b>Indicates the ZPU firmware status of the UmTRX  module:</b><br>OFF — The firmware is not loaded.<br>GREEN — The firmware is successfully loaded.</td></tr>
<tr><td> <b>LED F</b>    </td><td> <b>Indicates the FPGA firmware status of the UmTRX  module:</b><br>OFF — The firmware is not loaded.<br>GREEN — The firmware is successfully loaded.</td></tr></tbody></table>

At start-up, all 6 LEDs will flash and 2 will remain ON. LED F shows that the FPGA is configured. LED D shows that the firmware is running. The others are unused.<br>
<br>
LED3 line:<br>
<table><thead><th> <b>LED name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <b>LED H</b>    </td><td> <b>Unused</b><br>This LED is connected to the FPGA, but is not used currently.</td></tr>
<tr><td> <b>LED I</b>    </td><td> <b>Loss of Signal Indicator from the clock distributor:</b><br>OFF — Loss of clock signal occured.<br>GREEN — The clock signal is present.</td></tr>
<tr><td> <b>LED G</b>    </td><td> <b>Indicates the GPS module status:</b><br>OFF — The GPS module is switched off.<br>BLINKING — The GPS module has position fix.<br>GREEN — The GPS module is working.</td></tr></tbody></table>

Separate LEDs:<br>
<table><thead><th> <b>LED name</b> </th><th> <b>Description</b> </th></thead><tbody>
<tr><td> <b>LED4</b>     </td><td> <b>Indicates the FPGA memory configuration status:</b><br>Should light constantly during normal operation.<br>OFF — The configuration memory is not loaded.<br>RED — The configuration memory is loaded successfully.</td></tr>
<tr><td> <b>LED5</b>     </td><td> <b>Indicates the transmit status of the Ethernet PHY:</b><br>OFF — The Ethernet PHY is not transmitting data.<br>RED — The Ethernet PHY is transmitting data.</td></tr>
<tr><td> <b>LED6</b>     </td><td> <b>Indicates that the Ethernet PHY is operating in 1000Base-T mode:</b><br>OFF — The Ethernet PHY is operating in 10/100 Base-T mode.<br>RED — The Ethernet PHY is operating in 1000Base-T mode.</td></tr></tbody></table>

<h1>Power consumption and RF output power</h1>

Details are on a separate page: <a href='UmTRXv2_Power.md'>UmTRXv2 power</a>

<h1>UmSEL</h1>

UmSEL is a frontend for UmTRX which improves selectivity and blocker signal parameters when UmTRX is used as a GSM base station transceiver.<br>
<br>
<a href='UmSELv1_description.md'>Details about UmSEL operation</a>