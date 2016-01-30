# Introduction #

Big vendors typically design their BTS to exceed all requirements of the Standard by a certain margin, because they see this as a competitive advantage. This leads to extremely high price of their equipment. Our goal is very different - we're developing hardware with the optimal price/performance ratio.

UmTRX is designed as a multi-band and multi-standard transceiver and to meet RF requirements of different standards, it needs different RF frontends. E.g. requirements of GSM is very different from requirements of LTE and you can't satisfy both with at a reasonable cost.

UmSEL is designed to meet GSM 05.05 specification requirements for selectivity and blocking signals. UmSEL is a dual channel receiver with preselector, diversity switch, conversion and 1-st IF filtration (IF=360MHz). If UmTRX is equiped with UmSEL frontend, only single ARFCN could be received by each of UmTRX receiver channels. This limits wideband capabilities of UmTRX, but allows us to meet selectivity and blocking signal requirements of the GSM standard.

UmSEL also implements temperature control for UmTRX and four ADCs for voltage control test points, like output power detectors, DC/DC converters and so on.

UmSEL connects to UmTRXv2 board with the digital control connector as a mezzanine. RF signals are interconnected with short U\_FL/U\_FL RF cables.

# UmSELv1 specification: #

| PARAMETER | 900 MHz | 1800 MHz |
|:----------|:--------|:---------|
| Gain LNA to Switch output | 27 ± 1.5 dB | 27 ± 1.5 dB |
| Isolation LNA to Switch output | ~ 30 dB | 17..20 dB |
| Gain LNA to IF SAW output | 10 dB ± 1.5 dB | 10 dB ± 1.5 dB |
| IF SAW exact frequency | -->     | 359,95 MHz |
| Power consumption | -->     | 3.6 W (6V / 0.6A)|


# Solved problems of the GSM 05.05 requirements #

## Selectivity ##

LMS6002D has programmable zero-IF LPF filters for bandwidths 1.5 .. 28MHz. This is perfect for WCDMA/LTE signals, but is a problem for narrow-band GSM signals.

GSM requires narrow 270kHz filters and this is the most important issue solved by UmSEL. To meet GSM05.05 requirements we put on UmSEL high quality SAW filters at the 1st IF frequency.

## Noise figure ##

As known, internal LNA1 and LNA2 of LMS6002D has HF=3.5dB and 5dB and it is not bad.

For example, in the table below, you can see Rx front-end NF requirements for SNR=9dB and BER=10<sup>-3</sup>:

| | GSM900 | GSM900 | DCS1800 | DCS1800 |
|:|:-------|:-------|:--------|:--------|
| | MicroBS | NormalBS | MicroBS | NormalBS |
| Sensitivity, dBm | -100   | -104   | -102    | -104    |
| Noise factor, dB | 12     | 8      | 10      | 8       |

Lime Microsystems proposed LMS6002D chip for Femtocell and Picocell base stations.

Of course, we want Normal BS with sensitivity -107dBm, NF<2dB, SNR=12dB and BER=10<sup>-6</sup>, therefore high quality external LNA required, as it realized in UmSEL.

Also, onboard diversity switch of the UmSEL can improve upto 3dB for dynamic sensitivity in case of true diversity antennas.

## Linearity ##

Receiver linearity requirements determined by the intermodulation products of two or more signals or IP3, that mean virtual intercept point.

According to GSM 05.05 for NormalBS receiver sensitivity should be only 3dB above the referense sensitivity, i.e. -104 + 3 = -101 dBm when two blocking continious signals with 0.8MHz and 1.6MHz offsets comes to the BS's RF connector. For GSM900 allowed blockers upto -43dBm and for DCS1800 upto -49dBm.

So, input IP3 for NormalBS GSM900 should be at least -8dBm and for DCS1800 at least -17dBm.

For LMS6002D we can find that IIP3=-1dBm at medium gain, but it drastically falls at the high gain.

Higher IIP3 of LMS6002D can be reached if internal LNA's bypassed but used external high IIP3 LNA.

Due to this, in the UmSEL used high linear dual-stages LNA IC's MGA-13116 with IIP3=+5dBm for GSM900 and MGA-13216 with IIP3=+10dBm for DCS1800.

## LO phase noise ##
Here data of phase noise plot measured at 528 MHz:
| Offset, Hz | 1k | 10k | 100k | 400k | 600k |
|:-----------|:---|:----|:-----|:-----|:-----|
| Noise 1/Hz, dBc | -97 | -100 | -115 | -125 | -133 |

Seems it could be better...
So, don't dream it's over.