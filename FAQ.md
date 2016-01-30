**Table of contents**



# UmTRX questions #

## Mapping between RX1/TX1 label on a PCB and software channels ##

There are two LMS chips on UmTRX:
  * channel 1 (daughter board A) - RX1, TX1, SCN1 labels on a PCB
  * channel 2 (daughter board B) - RX2, TX2, SCN2 labels on a PCB

And each LMS chip has RX1, RX2, RX3 inputs and TX1, TX2 outputs ("antennas" in software) with the following mapping:
  * RX1 (software) = RXn (PCB label)
  * RX2 (software) = not connected on the PCB
  * RX3 (software) = SCNn (PCB label)
  * TX1 (software) = not connected on the PCB
  * TX2 (software) = TXn (PCB label)

# Fairwaves base station questions #

## 1. Could we use SIM-cards of other operators or we have to issue our own SIM-cards? ##

Fairwaves base station uses OpenBTS and could be configured to work with any SIM-cards. However, it’s impossible to provide subscriber authentication and traffic encryption if you don’t know Ki secret key of your SIM-cards.

If you operate a normal mobile network with encryption and subscriber authentication, you should have SIM-cards with known Ki keys. Usually this means that you buy SIM-cards to distribute them among you subscribers.


## 2. Do you have a solution for wide-band data, like 3G or 4G? ##

We believe that 3G is the dead end and the future will belong to 2G+4G+Wi-Fi networks.

We are offering Wi-Fi hot-spot for high-speed data coverage as a separated module. Also we are working on LTE eNodeB which could be used to provide 4G data access on fairwaves based networks.


## 3. Do you plan to develop a 3G solution? ##

See above. We decided to focus our development on 2G and 4G technology.


## 4. Assuming we purchase from you, is there technical support from your side? ##

UmTRX is an open-source project. Free support is provided through the community [mailing list](http://lists.osmocom.org/cgi-bin/mailman/listinfo/umtrx)

Commercial support will be provided for fairwaves solutions which may include UmTRX as a component.