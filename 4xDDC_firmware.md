# Introduction #

4xDDC firmware has 4 DSP cores and each DSP core can be configured to take data from channel A or channel B. UHD sees 4 virtual subdevices for channel A and 4 virtual subdevices for channel B.

4xDDC firmware is **Rx-only**, i.e. it doesn't support transmitting data. This is due to the FPGA size limitation - we can't fit a lot of Rx path processing and keep Tx path unchanged at the same time.

Since all virtual subdevices on side A and all virtual subdevices on side B share common LMS chips, setting RF frequency on one subdevice changes frequency for all other subdevices on the same side. To avoid this issue, you should set RF frequency only once for each side and change only DSP frequencies after that. In C++ you could achieve this by using POLICY\_MANUAL and POLICY\_NONE settings for tune requests:

```

//  For subdev (1)
    // tune the receiver with no cordic
    // RF center frequency is rx_lo_freq
    // DSP offset is rx_offset
    uhd::tune_request_t rx_tune_req();
    rx_tune_req.rf_freq_policy = uhd::tune_request_t::POLICY_MANUAL;
    rx_tune_req.rf_freq = rx_lo_freq;
    rx_tune_req.dsp_freq_policy = uhd::tune_request_t::POLICY_MANUAL;
    rx_tune_req.dsp_freq = rx_offset;
    usrp_1->set_rx_freq(rx_tune_req);

// get frequency LMS is tuned to:
    double real_lo = usrp-1->get_rx_freq();

...

//  For subdev (2), (3), (4)
    // don't touch center frequency, set DSP offset only
    // DSP offset is rx_offset
    uhd::tune_request_t rx_tune_req(rx_lo_freq);
    rx_tune_req.rf_freq_policy = uhd::tune_request_t::POLICY_NONE;
    rx_tune_req.rf_freq = 0;
    rx_tune_req.dsp_freq_policy = uhd::tune_request_t::POLICY_MANUAL;
    rx_tune_req.dsp_freq = rx_offset;
    usrp_2->set_rx_freq(rx_tune_req);

```

In gnuradio-companion similar behaviour could be achieved by subtracting DSP offset from the target frequency in a tune request. E.g. to set 2MHz offset for some channel, pass the following string to the "Ch X: Center Freq (HZ)" setting of a UHD sink block:
```
uhd.tune_request(var_freq - 2e6, 2e6)
```

# Installation instructions #

1. Flash your UmTRX with the [4xDDC ZPU firmware](http://people.osmocom.org/ipse/umtrx-v2-4ddc/current/usrp2p_txrx_uhd.bin) and the [4xDDC FPGA image](http://people.osmocom.org/ipse/umtrx-v2-4ddc/current/u2plus_umtrx_v2.bin). See [Flashing UmTRX](FlashingUmTRX.md) page for the flashing instructions.

2. Compile and install 4xDDC UHD version on your PC. To compile UHD you need to switch to [fairwaves/4xDDC](https://github.com/fairwaves/UHD-Fairwaves/tree/fairwaves/4xDDC) git branch and follow UHD normal build instructions.

NOTE: Currently there's no check if you use default ZPU or FPGA firmware images with the 4xDDC host library, or vice versa. Make sure versions of your ZPU, FPGA and host images match before reporting any bugs.

# LED indication #

LEDs meaning is different from the default firmware. In the 4xDDC version, we have 4 LEDs which indicate activity of relevant DSP cores:

|LED С | RX DSP 0|
|:-----|:--------|
|LED A | RX DSP 1|
|LED B | RX DSP 2|
|LED E | RX DSP 3|