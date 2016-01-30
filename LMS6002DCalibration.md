# Introduction #

LMS6002D should be calibrated to provide good results. Below we describe our own experience with LMS6002D calibration and peculiarities we don't want to forget. Hopefully it will be helpful for other LMS6002D users as well.

Most people start reading about the calibration procedures in the "Programming and Calibration Guide" and quickly find out description there is somewhat vague. A more hands-on description of calibration procedures could be found in the "EVB Quick Starter Manual" and thus it's a recommended reading even if you don't have an LMS EVB.

LMS6002D calibration involves several parts:

  1. DC calibration procedures:
    1. General DC calibration (automatic)
    1. DC Offset Calibration of LPF Tuning Module (automatic)
    1. Tx LPF DC Offset Calibration (automatic)
    1. Rx LPF DC Offset Calibration (automatic)
    1. RXVGA2 DC Offset Calibration (automatic)
  1. LPF Bandwidth Tuning (automatic)
  1. Tx LO leakage cancellation (manual)
  1. Tx I/Q balance calibration (manual)
  1. Tx PLL charge pump current `Icp` (manual)
  1. Rx PLL charge pump current `Icp` (manual)
  1. _... more to be added here._

# Automatic procedures #

## General DC calibration ##

From "EVB Quick Starter Manual", section 4.5.2:

> Carries out the top level DC calibration for the device, this is the R component of the RC cal value which is used in each of the LPF (Tx and Rx) process calibration values.

## LPF Bandwidth Tuning ##

From "EVB Quick Starter Manual", section 4.5.1 under "LPF Core Tuning":

> Executes the process related resistor capacitor (RC) calibration. LPF Core calibration is performed once per device to ensure that the corner frequencies of the LPFs are optimized. The calibration selects the LPF response which is closest and above the required bandwidth. This ensures modulation quality is not adversely impacted but sufficient rejection is provided for adjacent and alternate channel attenuation.

> This procedure should be run before any other Tx tuning/calibration, as optimum DC calibration values for LPF’s will change if this is done after the filter DC calibration.

## Tx LPF DC Offset Calibration ##

From "EVB Quick Starter Manual", section 4.5.1:

> This makes the DC contribution at output of filters zero so that DC level at the mixer input does not change when the TX VGA1 gain is changed.

> When executing this calibration make sure that no signal is applied to the transmit path. For better DC calibration low DC level signal can applied from baseband via DAC’s to transmit path.

## Rx LPF DC Offset Calibration and RXVGA2 DC Offset Calibration ##

From "EVB Quick Starter Manual", section 4.5.1:

> This minimizes the DC contribution at output of filters and Rx VGA2.

> When executing this calibration make sure that there is no signal applied to Rx input.

# Manual procedures #

## Tx LO leakage calibration ##

Described in section 4.8 of the "Programming and Calibration Guide" and in section 6.1 of the "EVB Quick Starter Manual".

Performed in LMS6002D.

Near the optimum point even change of calibration registers by 1 makes big difference. When you're far off, a change of a calibration register could be almost unnoticeable.

## Tx I/Q balance calibration ##

Described in section 4.10 of the "Programming and Calibration Guide" and in section 6.2 of the "EVB Quick Starter Manual".

Performed in baseband, i.e. in UHD in the UmTRX case.

UHD stores its I/Q compensation values in a file under the `${HOME}/.uhd/cal/` file under `*`nix and under the `%APPDATA%\.uhd\cal\` under Windows. With the current version of UHD the file name is `tx_iq_cal_v0.1_<serial>.csv`. The file name is constructed in the `apply_fe_corrections()` function in `lib/usrp/common/apply_corrections.cpp`.

UHD resets I/Q imbalance compensation values in the FPGA on startup, so you have to use this file to compensate I/Q imbalance. Setting those values manually before you run your application won't have any effect.

To manually calibrate your UmTRX:
  1. Copy this text into the aforementioned file:
```
name, TX Frontend Calibration
serial, 0
timestamp, 1335295846
version, 0, 1
DATA STARTS HERE
lo_frequency, correction_real, correction_imag, measured, delta
232.5e+06, -0.6, -0.55, 0, 0
```
  1. Tune LMS6002D to the desired frequency.
  1. Transmit 1MHz sine, e.g. using `tx_waveforms`. Write down level of the unwanted (lower) sideband.
  1. Change -0.6 and -0.55 values until you get the smallest level of the unwanted sideband. This will correspond to the minimum I/Q imbalance.

**ToDo**: This calibration could be done automatically using `uhd_cal_tx_iq_balance`, but to do that we have to (1) implement PLL tuning in UHD and (2) implement RF loopback controls for LMS.

## Tx PLL charge pump current `Icp` ##

**Control**: Register 0x16, default `Icp`=1200mA.

Charge pump current affects LMS' internal PLL phase noise and we found that `Icp`=1900mA provides the best Tx phase noise performance.