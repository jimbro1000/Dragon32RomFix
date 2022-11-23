# Dragon 32 Rom Adapter Fix #

This project details a simple conversion board to replace
two 8K masked ROMs with a single modern EEPROM.

The board is designed to accept 28 pin eeproms compatible with the
Atmel AT28C range.

## Direct (Twin) Rom Replacement ##

If the replacement ROM is a AT28C64 the board must be configured
for a 1:1 swap with one adapter for each ROM position. There is no
need to fit any of the SMT components or the 3 pin jumper.

If the replacement ROM is a AT28C256 the board needs to be 
assembled with the 3 pin jumper and a resistor at R1 and R3. The
jumper at JP2 must be bridged. This then allows the selection of a 
different ROM image by moving the JP1 jumper between positions 
(only when powered down). Only half of the ROM capacity will be 
used and the images should be loaded at $0000 and $4000. The
memory space between $2000-$3FFF and $6000-$7FFF cannot be
accessed in this mode without modifying the board.

### Bodging the Board ###

In order to fully utilise the available memory space in this
configuration it is necessary to find a way to drive pin 26 of
the eeprom by optionally inverting the signal to it.

The NAND gate position at U2 can be used as an inverter by taking 
the signal from pin 20 of the input socket and feeding it to J1. 
Some cutting of traces will also be needed to avoid
Putting a jumper or switch on that lead will allow you to 
switch between the two possible memory spaces (effectively 
giving you 4 selectable images). Ideally it should be a jumper 
with the "off" position going to pin 3 of JP1 (the pin furthest 
from R1).

## Single Rom Setup ##

If the replacement ROM is a AT28C256 the board can be configured 
for a 2:1 swap. In this arrangement it is essential to fully
populate the board with all 3 resistors and the NAND gate. A fly
lead from J1 is also needed.

In this arrangement the board should be fitted to position IC18
with the flylead going to pin 20 of IC17.

JP1 still allows swapping between two different ROM images but
all of the memory space on the ROM will be utilised. The first
image is from $0000-$3FFF and the second image is from $4000-$7FFF.

It is possible to fit the board in position IC17 but the ROM images
will need resequencing:

$0000-$1FFF => ROM1 1-1  
$2000-$3FFF => ROM1 1-0  
$4000-$5FFF => ROM2 1-1  
$6000-$7FFF => ROM2 1-0  

# BOM #

| Position | Component |
| -------- | --------- |
| R1, R2, R3 | 4K7 0805 |
| U1 | 2 x 12 position 2.54mm pin header | 
| U2 | 74LVC1G00 SOT-353 | 
| U3 | DIP-28 socket | 
| JP1 | 3-pin right-angle 2.54mm pin header |

The resistor values given are recommended - all 3 are acting as 
pull-up or pull-down purposes so smaller or larger values can be
used as appropriate. It is also possible to simply solder bridge 
the positions but this has potential to damage the EEPROM.

For the U1 pin headers I recommend using turned-pins *and* fitting 
a turned-pin socket on the main board to ensure a quality fit and 
no damage from repeated insertions. Using square pins will 
ultimately damage a regular single- or double-wipe socket but it
is possible to fit female pin headers to ensure compatibility but
this then prevents putting regular (original) Dragon ROMs back in.
