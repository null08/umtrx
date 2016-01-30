# Milestone "Congress" aka "The Prototype" #

Planned for 28th Chaos Communication Congress, 27-30 December 2011, bcc, Berlin, Germany.

## Hardware ##

  * ~~Power connectors for fan~~
  * ~~Filters bypass with 0-ohm resistor~~
  * ~~Check .ucf versus schematics~~
  * ~~More test points for LMS~~
  * ~~More test points for FGPA~~
  * ~~Change 1GbE clock to [this TCXO](http://www.sitime.com/products/temperature-compensated-vctcxo/sit5002) or [this TCXO](http://search.digikey.com/us/en/products/ASFLMB-25.000MHZ-LR-T/535-10992-1-ND/2624467)? NO~~
  * ~~Power from 12V for solar panels/batteries?~~
  * ~~Temperature control. [MAX6665](http://www.maxim-ic.com/datasheet/index.mvp/id/2579) may be a good fit for this.~~

# Milestone "Mayotte" #

2013 Spring

## Hardware ##

  * ~~Replace EB230 GPS module with EB500/EB570.~~
  * Inverse polarity power protection
  * Add watchdogs and low-power-swtch-offs: I read this presentation by Eric Brewer (TIER group leader): http://www.citris-uc.org/files/EricBrewer2008.pdf and what I particularly noticed is that they put a lot of attention to power issues in rural areas and importance of hardware abd software watchdogs and low-power-switch-off units wihtout them they noticed handful of flash FS crashes and "weird" WiFi problems. I think it's an important observation which should be taken into consideration during MP development. Also they say there are low-cost solar power controllers in works, which was also discussed on this mailing list some time ago.
  * ~~Is selectivity a big problem for us?~~
  * "Site I/O" GPIO which can be controlled via OML.  Some of them might have opto-couplers or even relay contacts.  The idea here is to have things like "intrusion detection" or "fuel tank for generator is running empty", etc. be reported via the BTS to the BSC and finally the OMC. Option: an off-the-shelf USB/IO board, connected to a PC.
  * Add connectors for LMS RXOUT (just in case we want to use them to measure two external analog signals)
  * ~~Connector for JTAG on ET1011C2?~~
  * ~~ESD protection~~
  * lightning protection
  * Add self-testing features
  * ~~Tx and Rx ports protection. GPS and CLK ports protection.~~

## Software ##

  * Software watchdogs
  * Read-only FS - with aufs? https://help.ubuntu.com/community/aufsRootFileSystemOnUsbFlash

# Random ideas #

  * **Power-over-Ethernet** support for UmTRX without an external PA.
  * **Multi-ARFCN: Categorize assignement by "power"**, so that in the same timeslots you have similar power levels. This will allow to minimize problems with strong MS signal blocking weak MS signal.
  * ~~**Multi-ARFCN: Improve LMS's Rx dynamic range** during multi-ARFCN operation by utilizing two LMS chips. Both chips are tuned to the same frequency, one chip is set to higher input gain and the other is set to lower input gain. This way we can capture both weak and strong signals equally.~~<br />    This is an interesting solution when we need a single TRX with multi-ARFCN, but I see a problem with it. When one ARFCN has strong signal while others have weak signals, we can receive only ARFCN with the strong signal. LMS with higher input gain will be saturated and useless, while LMS with lower input gain will not see weak signals at all.<br />    _Andrey Sviyazov: There is a big chance this won't work. Frequency synthesizers will interfere with each other if they will work on the same frequency. Phase beats will arise between them or worse they will knock each other out of the phase (frequency) lock. We did synthesizers in separate aluminum housings and were able to get them to work well on a some frequency only when used filters on all of the control circuits. I think to realize this on a single PCB would be very difficult or impossible. I have provided the filters to eliminate the mutual influences between PFD's of synthesizers via signal 26 MHz, but this is not enough._
  * ~~**Diversity: Switch from switched to full diversity** when dynamic range is ok to accommodate both ARFCNs. If both ARFCNs in a cell are split by <1MHz, then we can configure LMS chips to capture either one ARFCN each or both ARFCNs at the same time. If conditions require high dynamic range then each LMS should capture one ARFCN, otherwise both LMSes can capture both ARFCNs and we can benefit from full diversity.~~ <br />    _The problem with the previous idea undermines this idea as well._