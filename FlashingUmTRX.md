# Flashing UmTRX #

Stable builds of the FPGA and ZPU images are always available at [people.osmocom.org](http://people.osmocom.org/ipse/umtrx-v2/current/).

## Ethernet flashing ##

Flashing over Ethernet is the easiest way to update firmware of your UmTRX. It can be performed in both production and [safe](BootingAndSafeMode.md) modes.

**Production firmware**

Flash both FPGA and ZPU images:
```
cd host/utils
./usrp_n2xx_net_burner.py --addr=<ip address> --fpga=u2plus_umtrx_v2.bin --fw=usrp2p_txrx_uhd.bin --reset
```

This command will reset your UmTRX after flashing to immediately boot into the updated firmware.

**Safe firmware**

**WARNING**: Don't do this unless you have a JTAG cable, you could brick your UmTRX.
```
cd host/utils
./usrp_n2xx_net_burner.py --addr=<ip address> --fpga=u2plus_umtrx_v2.bin --reset --overwrite-safe
```


## JTAG flashing ##

We can program FPGA and SPI flash using a tiny utility called xc3sprog (http://xc3sprog.sourceforge.net/). It's available under Linux and Windows, We tested it only under Linux with Digilent HS-1 cable.  Note: only the latest svn version supports this cable.

To program FPGA without writing to flash, i.e. the program will be erased after power cycling:
```
sudo ./xc3sprog -c jtaghs1 u2plus_umtrx.bit
```

To reset FPGA:
```
sudo ./xc3sprog -c jtaghs1 -R
```

To write to SPI flash from a given address (`0` in this example). `s6slx75_fgg484.bit` file is not shipped with `xc3sprog`, you could get it at the [Downloads section of this project](https://code.google.com/p/umtrx/downloads/detail?name=s6slx75_fgg484.bit).
```
sudo ./xc3sprog -c jtaghs1 -Is6slx75_fgg484.bit u2plus_umtrx.bit:w:0:BIT
```