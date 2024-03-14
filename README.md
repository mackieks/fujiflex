<picture> <source media="(prefers-color-scheme: dark)" srcset="images/logo.png"> <img src="images/logo_black.png" width="200"> </picture> 

OSHW GCVideo DVI flex for Nintendo Wii. Tested and working! Solderpad Hardware License v2.1

<img src="https://github.com/mackieks/fujiflex/blob/main/images/fujiflex.png" width=1200>



## fujiflex features
- [x] Compact 28 x 30mm 2-layer flex PCB (requires KiCAD 8.0RC1 or later)
- [x] Powered from 3.3V and 1.8V (1.2V generated from 1.8V)
- [x] 19-pin Molex 5052781933 ZIF for HDMI output
      
**Note:** As of 2024-03-06, Molex 5052781933 is EoL. Replacement PN is 5052781970, available ~Q3 2024
- [x] Wii SDA and SCL testpoints
- [x] MODE solder jumper
- [x] Powered by [Ingo Korb's](https://github.com/ikorb) [GCVideo DVI](https://github.com/ikorb/gcvideo/)

<img src="https://github.com/mackieks/fujiflex/blob/main/images/layout.PNG" width=800>

## Assembly

- Apply solder paste to the board using your stencil
- Place the components (except R6 if flashing in-system)
- Reflow the board using a hot plate
- Inspect the board, check for bridges and shorts

## Flashing GCVideo

Once assembled, you'll need to flash the GCVideo firmware. The most straightforward approach is to program *in-system* using an SPI flash programmer such as an EZP20xx or CH341A-based board connected directly to pads on fujiflex.

- Remove R6 to avoid powering the FPGA during programming
- Wire the CS, MISO, MOSI, CLK, GND pads to the corresponding pins on your SPI flash programmer
- Wire the rightmost pad of R6 to 3.3V on your SPI flash programmer
- Download the *shuriken-v3-wii* firmware from [GCVideo's latest release](https://github.com/ikorb/gcvideo/releases/latest)
- Flash the firmware file (eg. `gcvideo-dvi-shuriken-v3-wii-3.1-spirom-complete.bin`) to the SPI chip (see below)
- Re-populate R6

![fujiflex pads](images/fujiflex_pads.png)

### EZP20xx programmers

If you are using an EZP20xx programmer, you can flash GCVideo using the [Windows software](https://www.chipsetpro.com/en/content/7-download-software) for these devices.

### Other programmers (using `flashrom`)

For other programmers, [flashrom](https://flashrom.org) is an open source command line tool for programming flash chips which supports [tons of devices](https://wiki.flashrom.org/Supported_programmers), including the inexpensive CH341A-based boards.

- First expand the GCVideo firmware to match the size of the SPI flash chip:

    ```shell
    fallocate -l 512k gcvideo-dvi-shuriken-v3-wii-3.1-spirom-complete.bin
    ````

- Then flash the firmware, for example with a CH341A programmer:

    ```shell
    flashrom --programmer ch341a_spi -w gcvideo-dvi-shuriken-v3-wii-3.1-spirom-complete.bin
    ```

### Socket adapters

If you are assembling multiple boards, you may find it quicker and easier to to program the flash chip before populating it on the board using a [suitable adapter socket](https://www.aliexpress.us/item/3256802266989931.html).

## Installation

fujiflex is soldered directly to the [digital A/V vias](https://github.com/ikorb/gcvideo/blob/main/HDL/gcvideo_dvi/README-Wii.md#digital-audio-and-video) on a 4-layer Wii motherboard.

- Align and solder fujiflex to the A/V vias
- Wire the 1.8V, 3.3V, 5V, and GND pads
- To use a Gamecube controller for navigating the GCVideo OSD, you'll also need to [wire the GCC pad](https://github.com/ikorb/gcvideo/blob/main/HDL/gcvideo_dvi/README-Wii.md#controller)

## License

Solderpad Hardware License v2.1