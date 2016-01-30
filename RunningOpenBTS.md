# Hardware setup #

  * Connect UmTRX to a 1GbE port of a computer running OpenBTS. Note, that UmTRX doesn't support 100Mbit Ethernet connection yet and thus will not be recognized if your computer doesn't hae 1GbE connection.
  * By default UmTRX has a static IP address 192.168.10.2/24, so we recommend you to set your computer's IP address to 192.168.10.1/24.
  * _TODO: antennas setup_

# Software setup #

## Download ##

  * UHD: [git://github.com/fairwaves/UHD-Fairwaves.git fairwaves/umtrx](https://github.com/fairwaves/UHD-Fairwaves)
  * OpenBTS: [git://github.com/fairwaves/openbts-2.8 umtrx](https://github.com/fairwaves/openbts-2.8/tree/umtrx)

**Note: UmTRX currently does NOT work with the stock version of UHD. You have to use the UHD version mentioned above.**

## Build ##

Follow standard build instructions.

  * UHD: http://files.ettus.com/uhd_docs/manual/html/build.html
  * OpenBTS: http://wush.net/trac/rangepublic/wiki/BuildInstallRun

## Run OpenBTS ##

Follow standard instructions.

> http://wush.net/trac/rangepublic/wiki/BuildInstallRun

## Tune OpenBTS for specific hardware ##

If you want to get the best coverage from your OpenBTS setup, you must be sure your cell has good UL/DL balance. I.e. it should not be UL- or DL-limited. For this purpose OpenBTS has several parameters which should be tuned for every specific hardware.

Note: to follow the following instructions you should have a working OpenBTS setup with at least one connected phone.

  1. **Run "`noise`"** command in the OpenBTS CLI several times to get the worst (=biggest) value of noise RSSI for your setup. It should be a negative value, like -44dB.
```
OpenBTS> noise
noise RSSI is -44 dB wrt full scale
MS RSSI target is -37 dB wrt full scale
```
  1. Add 6-8dB to the noise RSSI value to get the RSSI level we need to receive phone signal on the BTS without errors. This is called **"target RSSI"** in OpenBTS and is set with "`GSM.Radio.RSSITarget`" value in the config. If noise RSSI is -44dB, then you could set target RSSI to -37dB:
```
OpenBTS> config GSM.Radio.RSSITarget -37
```
  1. **Make a call** from your test mobile to another mobile or a soft-switch. During the next steps we will use active channel between the mobile and the base station to estimate link quality and tune Rx gain and Tx attenuation.
  1. **Adjust "`rxgain`"** to provide good reception at the edge of the bae station reception area and at the same time to avoid saturation (and thus bad reception) close to the base station. The easiest way to estimate reception quality is to use "`UPFER`" value at the "`chans`" CLI command output. Zero "`UPFER`" means good reception, non-zero "`UPFER`" means errors on UL channel:
```
OpenBTS> chans
CN TN chan      transaction UPFER RSSI TXPWR TXTA DNLEV DNBER
CN TN type      id          pct    dB   dBm  sym   dBm   pct
 0 1    TCH/F   1788614112  0.00   -5     5    0  -101  0.00
                            ^^^^
                        Good UL quality!

OpenBTS> chans
CN TN chan      transaction UPFER RSSI TXPWR TXTA DNLEV DNBER
CN TN type      id          pct    dB   dBm  sym   dBm   pct
 0 1    TCH/F     22843555 56.80  -27     5    0   -59  0.00
                           ^^^^^
                        Bad UL reception.

```
  1. "`power`" CLI command sets attenuation of the Tx power relative to the maximum base station power. It has two values ("`max`" and "`min`", but for a small base station it's easier to set them to the same value). Place the test mobile phone at the edge of good reception area and **increase "`power`"** value until "`DNBER`" starts to increase. It should happen when "`DNLEV`" reaches around -105dB.
```
OpenBTS> power 55 55
current downlink power -50 dB wrt full scale
current attenuation bounds 50 to 50 dB
new attenuation bounds 55 to 55 dB

OpenBTS> chans
CN TN chan      transaction UPFER RSSI TXPWR TXTA DNLEV DNBER
CN TN type      id          pct    dB   dBm  sym   dBm   pct
 0 1    TCH/F   1788614112  0.00   -5     5    0  -107  2.26
                                                  ^^^^^^^^^^^
                                               DL signal is too weak.
```
  1. If you need **to limit the radius** of your base station, it's better to do by decreasing the base station Tx power, i.e. by increasing attenuation with the "`power`" command.