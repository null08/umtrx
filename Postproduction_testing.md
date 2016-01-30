Basic checks to perform after production to make sure there are no critical issues with manufacturing. FOr our own manufacturing we use an extended version of this test with much more tests to check correct operation.

# Post-production testing #

## Required instruments: ##

  1. Laboratory Power Supply Unit (PSU) 3-15V/0-2A with DC current indication or external ampere meter.
  1. voltmeter or multimeter with good accuracy.
  1. spectrum analyzer at least like Signal Hound.
  1. PC with 1GbE port, configured with IP 192.168.10.1/24.
  1. 1GbE capable cable, plugged into the PC port described above.
  1. RF cables, adapters, attenuators etc.

## UHD version ##

To work with UmTRX you should download and build a special version of UHD from [our github](https://github.com/chemeris/UHD-Fairwaves):
```
git clone git://github.com/chemeris/UHD-Fairwaves.git
```

Then follow [normal UHD build instructions](http://files.ettus.com/uhd_docs/manual/html/build.html). You only need to build 'host' part of UHD.

## Testing ##

<ol>

<li> <b>Critical issues</b><br />
Check that all <a href='https://code.google.com/p/umtrx/issues/list?can=2&q=Priority%3DCritical'>critical issues</a> are fixed or worked around for the unit under the test.<br>
<br>
<li> <b>Short circuit</b><br />
Adjust PSU current limit to 1..1.5A, set voltage to 3..4V and double check polarity. Apply power to UmTRX and check UmTRX current consumption with the PSU:<br>
<br>
<ul><li>If the current consumption of UmTRX is limited by the PSU, check the power polarity. If the polarity is correct, mark this UmTRX unit as defective with the "over-current" label.</li></li></ul>

<ul><li>If the current consumption is below the limit, slowly increase PSU output voltage to 12V. Current consumption should be less then 0.5A at 12V - probably around 0.3A (without Ethernet cable connected and without firmware loaded).</li></ul>

<li> <b>DC-DC converters</b><br />
Test voltages of all the following test points:<br>
<table><thead><th> C140 </th><th> 2.5V </th></thead><tbody>
<tr><td> C138 </td><td> 1.2V </td></tr>
<tr><td> C141 </td><td> 3.3V </td></tr>
<tr><td> C131 </td><td> 6V   </td></tr>
<tr><td> U18  </td><td> 5V   </td></tr>
<tr><td> U28  </td><td> 3.3V </td></tr>
<tr><td> U29-1, U29-2 </td><td> 3.3V </td></tr>
<tr><td> U30-1, U30-2 </td><td> 2.5V </td></tr>
<tr><td> U31-1, U31-2,<br />U32-1, U32-2 </td><td> 1.8V </td></tr>
<tr><td> Collector Q3<br /> near GBEth </td><td> 1.0V </td></tr></tbody></table>

<li> <b>Clock source</b><br />
Check that both switches of the S3 dip-switch are turned down (position "master"). Check that LED-I lights constantly (indicates that clock is ok) and LED-G lights constantly (indicates that GPS is powered on).<br>
<br>
<li> <b>Ethernet PHY</b><br />
Plug in an Ethernet cable, connected to a 1GbE network.<br>
<br>
<ul><li>Current draw should increase by about 100-150mA.</li></ul>

<ul><li>LED6 and the right LED on the Ethernet connector should light constantly, LED5 and the left LED on the Ethernet connector might blink.</li></ul>

<li> <b>FPGA firmware</b><br />
Switch off power. Connect JTAG adapter. Switch on power back. Load firmware using <a href='BuildingUHD#Flashing_FPGA.md'>this manual</a>. <font color='red'>Describe LED status, debug port output</font> Firmware images:<br>
<br>
<ul><li><a href='http://people.osmocom.org/ipse/umtrx-v2/current/'>UmTRXv2</a></li></ul>

<li> <b>GPS sync</b><br />
Connect GPS antenna and wait for ~1 min for LED G to start blinking - this means that GPS sync is acquired.<br>
<br>
<li> <b>LEDs status</b><br />
Check all LED's in according to functionality as described in <a href='FeaturesAndTechnicalSpecification.md'>FeaturesAndTechnicalSpecification</a>.<br>
<br>
<li> <b>LMS init</b><br />
<font color='red'>Describe for both LMS chips</font><br />
Connect spectrum analyzer (SA) to TX1 output and adjust SA and TX at the same frequency, 960MHz for example. Found TxLO leakage level.<br>
<br>To check LMS's control lines try to play with adjustments of LMS's.<br>
<br>
Usual contents of <b>.bat</b> file to init basic functions:<br>
<pre><code>set lms=%C:\Python32\UmTRX\umtrx_lms.py --umtrx-addr 192.168.10.2 --lms 1%<br>
%LMS% --lms-init<br>
%LMS% --lms-tx-enable 1<br>
%LMS% --lms-rx-enable 1<br>
%LMS% --lms-set-tx-vga1-gain -10<br>
%LMS%  --pll-ref-clock 26e6 --lpf-bandwidth-code 0x0f --lms-auto-calibration<br>
%LMS% --lms-tx-pll-tune 960000000<br>
%LMS% --lms-set-tx-pa 2<br>
%LMS% --lms-set-tx-vga2-gain 20<br>
%LMS%  --lms-rx-pll-tune 915000000<br>
%LMS% --lms-set-rx-vga2-gain 9<br>
%LMS% --lms-set-rx-lna 3<br>
</code></pre>

<li> <b>Modulated Tx</b><br />
Make modulated signal and check power consumption in dependency of functionality as described in <a href='UmTRXv2_Power.md'>UmTRXv2_Power</a>.<br>
<br>Be carefully because of modulated signal of UmTRX will be much higher then TxLO leakage and can damage your SA.<br>
<br>
To get ideal GMSK modulation might be used the next example:<br>
<pre><code>tx_samples_from_file.exe --rate 1083333 --freq 9478e5 --file gmsk.cfile --loop<br>
</code></pre>
To make measurements of phase error etc, better to <a href='RunningOpenBTS.md'>RunningOpenBTS</a> after <a href='LMS6002DCalibration.md'>LMS6002DCalibration</a>.