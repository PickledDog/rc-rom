# rc-rom
 ROM card for RC2014
![Assembled PD104](/img/assembled.jpg)
(version 1.0 shown, current version 1.1 has a second LED for paging status)

## Overview
This RC2014-compatible board provides ROM from an EPROM or EEPROM, with 8k/16k/32k granularity. The ROM can be paged out and replaced by RAM, when combined with a compatible RAM card. A user-controllable LED is also provided.

## ROM
This board provides ROM to a RC2014 system. It does not provide RAM, but can interact with a compatible RAM card. The [official RC2014 64K RAM](https://rc2014.co.uk/modules/64k-ram/) card is supported, as is the [PD101 board](https://github.com/PickledDog/rc-z80ram), which it was designed in tandem with.

## Paging
Paging (the ability to replace the ROM with RAM) is implemented using the *PAGE* signal in the RC2014 Enhanced Bus. This is the only Enhanced Bus signal required - if you're using a Standard Bus backplane (or don't intend to install a two-row header), a wire can be used to link the page signals between this board and the RAM (the pin in question is #4 of the 10 Enhanced pins).

If the *PAGE* signal cannot be connected, any RAM in the system will need to be configured to not appear in the lower memory addresses used by the ROM - consult the documentation of the RAM board. PD101 *cannot* selectively disable memory like this, and requires the PAGE signal in order to work with this board.

## Software compatibility
The board presents two I/O devices to the system - the LED at base port 0x08, and the ROM paging at 0x38. The LED appears at the same address as the LED of the [SC114 Motherboard](https://smallcomputercentral.wordpress.com/sc114-documentation/), and the ROM control address is common to most Z80 retrocomputers (the official RC2014 RAM/ROM cards included). As such, this board is compatible with most SC114 software, as well as Grant Searle's CP/M loader. LED is active low (lights by default) - writing a **1** to 0x08 turns it off. Writing a **1** to 0x38 disables the ROM.

## Chip support
The board supports 27C64, 27C128, 27C256, and 27C512 EPROMs, as well as 28C64 and 28C256 EEPROMs. Other chips with similar pinouts will *probably* work, but are untested. 150ns parts (like most 28C256s) are running at the edge of their specification in a standard 7.3Mhz system, and may require lowering the CPU clock. The [27C256](https://www.mouser.com/ProductDetail/AT27C256R-70PU/), [27C512](https://www.mouser.com/ProductDetail/AT27C512R-70PU/), and [28C256](https://www.mouser.com/ProductDetail/AT28C256-15PU/) can be purchased new.

## Firmware
Most (if not all) [RC2014 ROM images](https://github.com/RC2014Z80/RC2014/tree/master/ROMs) can be used unmodified (selecting the right images for your serial board, of course). Stephen C Cousins' excellent [Small Computer Monitor](https://smallcomputercentral.wordpress.com/small-computer-monitor/) can be used as well - all of the R* configurations are supported. You will need an EPROM programmer (for example, a [TL866](https://www.ebay.com/sch/i.html?_nkw=tl866ii+plus)) to load the ROM images onto a chip.

## Jumper settings
Refer to the front of the board for detailed jumper setting instructions.

![Jumpers](/img/jumpers.jpg) ![Settings](/img/settings.jpg)

## Part selection
Bill Of Materials and part references are below. In order to make the 50-pin RC2014 Enhanced bus connector, you'll need to take a 2x40-pin right-angle header and pull out most of the pins from the top row (needle-nose pliers work well). I recommend using gold-plated header for this - I use these ones from [Pololu](https://www.pololu.com/product/2668) or [Sparkfun](https://www.sparkfun.com/products/12792). The jumper headers can be snapped from regular (or double-row) breakaway header, for which eBay is substantially cheaper; the Mouser listings are provided for convenience.

The specified parts are just the ones I used, and can be substituted as needed - Mouser links provided for convenience and reference. A ZIF socket can be used for the EPROM - these are best [sourced from eBay](https://www.ebay.com/sch/i.html?_nkw=28+pin+zif+socket).

| Reference | Value | Qty | Mouser link |
| --------- | ----- | --- | ----------- |
| C1-C4 | 100nF ceramic | 4 | [KEMET C315C104M5U5TA](https://www.mouser.com/ProductDetail/C315C104M5U5TA7303) |
| D1 | 3mm yellow LED | 1 | [Lite-On LTL-4251N](https://www.mouser.com/ProductDetail/LTL-4251N) |
| D2 | 3mm green LED | 1 | [Lite-On LTL-4231N](https://www.mouser.com/ProductDetail/LTL-4231N) |
| J1/2 | 2x40 right-angle header | 1 | [3M 2380-5121TG](https://www.mouser.com/ProductDetail/2380-5121TG) |
| JP1/2 | 2x3 header | 1 | [Amphenol 10129381-906002BLF](https://www.mouser.com/ProductDetail/10129381-906002BLF) |
| JP3/4/5 | 3x3 header | 1 | [Samtec TSW-103-07-L-T](https://www.mouser.com/ProductDetail/TSW-103-07-L-T) |
| JP6 | 2x2 header | 1 | [Amphenol 10129381-904002BLF](https://www.mouser.com/ProductDetail/10129381-904002BLF) |
| | jumpers | 7 | [Harwin M7583-46](https://www.mouser.com/ProductDetail/M7583-46)
| R1, R2 | 10kΩ resistor | 2 | [Yageo CFR-25JT-52-10K](https://www.mouser.com/ProductDetail/CFR-25JT-52-10K) |
| R3, R4 | 470Ω resistor | 2 | [Yageo CFR-25JT-52-470R](https://www.mouser.com/ProductDetail/CFR-25JT-52-470R) |
| U1 | EPROM | 1 | (see above for EPROM choices) |
| | socket | 1 | [Amphenol DILB28P-223TLF](https://www.mouser.com/ProductDetail/DILB28P-223TLF), or above ZIF |
| U2 | 74HCT138 | 1 | [TI SN74HCT138N](https://www.mouser.com/ProductDetail/SN74HCT138N) |
| U3 | 74HCT74 | 1 | [TI SN74HCT74N](https://www.mouser.com/ProductDetail/SN74HCT74N) |
| U4 | 74HCT4075 | 1 | [TI CD74HCT4075E](https://www.mouser.com/ProductDetail/CD74HCT4075E) |