**Table of contents:**


# Boot sequence #

UmTRX stores 3 files in its flash which are used for booting up:
  * **Safe FPGA** image with built in ZPU bootloader.
  * **Production FPGA** image with built in ZPU bootloader.
  * **Production ZPU** image.

**Simplified boot order** is as follows:
  1. Safe FPGA image.
  1. ZPU bootloader (from the safe FPGA image).
  1. Production FPGA image.
  1. ZPU bootloader (from the production FPGA image).
  1. Production ZPU image.

# Entering the safe mode #

<a href='http://wiki.umtrx.googlecode.com/hg/images/documentation/safe-reset-buttons.jpg'><img src='http://wiki.umtrx.googlecode.com/hg/images/documentation/safe-reset-buttons.jpg' align='right' width='150px' /></a>
Safe mode is useful when you flashed a wrong or buggy FPGA image or assigned wrong IP or MAC addresses to UmTRX. In this case you could enter safe mode and [flash a proper FPGA and ZPU images to UmTRX](FlashingUmTRX.md), or just use UmTRX as in normal mode.

In safe mode UmTRX loads a safe FPGA image and safe ZPU firmware which are stored separately in its flash. Safe FPGA image has the same functionality as a production one and differ only in their location in the flash. In other words, the same FPGA image could be considered safe and production depending on its location in the flash.

Follow this procedure to enter safe mode:
  1. Press and hold "Safe" button.
  1. Power cycle UmTRX or press "Reset" button.
  1. Wait for LEDs D and F to light constantly and release "Safe" button.
  1. You're in safe mode!

# What is a safe mode, exactly? #

Safe mode has a number of features which make it so useful. Most importantly:

  * Safe FPGA firmware is loaded instead of a production one.
  * Safe ZPU firmware is loaded instead of a production one.
  * IP address is set to 192.168.10.2 instead of the one from EEPROM.
  * MAC address is set to 00:50:C2:85:3f:ff instead of the one from EEPROM.

# Appendix: Complete boot order #

  1. Safe FPGA image
    1. All LEDs turned off
    1. FPGA loads safe image.
    1. On failure:
      * stop
    1. On success:
      * LEDs D lights
      * continue with loading a ZPU bootloader
  1. ZPU bootloader (from the safe FPGA image).
    1. FPGA loads ZPU bootloader.
    1. LEDs A, C, E flash.
    1. LED D turns on.
    1. If "Safe" button is pressed:
      * interrupt boot and enter normal operation
    1. The bootloader checks presence of a valid FPGA production image.
    1. If not present:
      1. Check presence of a valid production ZPU image.
      1. If not present:
        * interrupt boot and enter normal operation
      1. If not present:
        * continue with loading the production ZPU image
    1. If present:
      * continue with loading the FPGA production image
  1. Production FPGA image.
    1. All LEDs turned off
    1. FPGA loads safe image.
    1. On failure:
      * stop
    1. On success:
      * LEDs D lights
      * continue with loading a ZPU bootloader
  1. ZPU bootloader (from the production FPGA image).
    1. FPGA loads ZPU bootloader.
    1. LEDs A, C, E flash.
    1. LED D turns on.
    1. Check presence of a valid production ZPU image.
    1. If not present:
      * interrupt boot and enter normal operation
    1. If not present:
      * continue with loading the production ZPU image
  1. Production ZPU image.
    1. FPGA loads ZPU bootloader.
    1. LEDs A, C, E flash.
    1. LED D turns on.