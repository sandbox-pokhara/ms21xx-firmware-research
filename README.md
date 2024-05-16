# MacroSilicon MS21XX EEPROM Firmware Research

## Files

- [Original Fimware Dumped from MS2109](MS2109-CLEAN-FIRMWARE.bin)
- [Original Fimware Dumped from MS2130](MS2130-CLEAN-FIRMWARE.bin)

## Data Format (MS2109)

| Bytes (HEX)  | Label          | Example Values                                    | Descrption                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------ | -------------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 00-01        | EEPROM Type    | `A5 5A` or `96 69`                                | `A5 5A` for 24c01/02/04/08/16, `96 69` for 24c32/64                                                                                                                                                                                                                                                                                                                                                      |
| 02-03        | Code Length    | `06 BC`                                           | Length of code in bytes, the code starts from 30, after Audio (USB)                                                                                                                                                                                                                                                                                                                                      |
| 06-07        | VID (USB)      | `53 4D`                                           | 2 bytes Vendor ID, `53 4D` is Macro Silicon Vendor ID                                                                                                                                                                                                                                                                                                                                                    |
| 08-09        | PID (USB)      | `21 09`                                           | 2 bytes Product ID, `21 09` is the product id of MS2109                                                                                                                                                                                                                                                                                                                                                  |
| 0C-0F        | Version        | `20 07 29 01`                                     | Version of the firmware                                                                                                                                                                                                                                                                                                                                                                                  |
| 10-1F        | Video (USB)    | `0A 55 53 42 20 56 69 64 65 6F FF FF FF FF FF FF` | First byte is the size of string, follwed by data, example value translates to USB Video                                                                                                                                                                                                                                                                                                                 |
| 20-2F        | Audio (USB)    | `0A 55 53 42 20 41 75 64 69 6F FF FF FF FF FF FF` | First byte is the size of string, follwed by data, example value translates to USB Audio                                                                                                                                                                                                                                                                                                                 |
|              | EEID (Monitor) |                                                   | EEID can be used to change the monitor manufacturer name and serial number, product type, capabilities, etc. EEID is in an arbitary position, search for the header `00 FF FF FF FF FF FF 00` followed by data, which is 256 bytes including the headers. More details on EEID data format can be found [here](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#EDID_1.4_data_format). |
| Last 4 bytes | Checksum       | `27 02 52 8D`                                     | The checksum data comes right after code ends. First two bytes, `27 02` is the checksum of bytes `02-2F` and last two bytes, `52 8D` is the checksum of code.                                                                                                                                                                                                                                            |

## Data Format (MS2130)

| Bytes (HEX)  | Label          | Example Values                                    | Descrption                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------ | -------------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 00-01        | EEPROM Type    | `5A A5` or `69 96` or `3C C3`                     | `5A A5` for 24c01/02/04/08/16, `69 96` for 24c32/64, `3C C3` for Flash                                                                                                                                                                                                                                                                                                                                   |
| 02-03        | Code Length    | `06 BC`                                           | Length of code in bytes, the code starts from 30, after Audio (USB)                                                                                                                                                                                                                                                                                                                                      |
| 04-05        | VID (USB)      | `34 5F`                                           | 2 bytes Vendor ID, `34 5F` is Macro Silicon Vendor ID                                                                                                                                                                                                                                                                                                                                                    |
| 06-07        | PID (USB)      | `21 30`                                           | 2 bytes Product ID, `21 30` is the product id of MS2109                                                                                                                                                                                                                                                                                                                                                  |
| 0C-0F        | Version        | `20 07 29 01`                                     | Version of the firmware                                                                                                                                                                                                                                                                                                                                                                                  |
| 10-1F        | Video (USB)    | `0A 55 53 42 20 56 69 64 65 6F FF FF FF FF FF FF` | First byte is the size of string, follwed by data, example value translates to USB Video                                                                                                                                                                                                                                                                                                                 |
| 20-2F        | Audio (USB)    | `0A 55 53 42 20 41 75 64 69 6F FF FF FF FF FF FF` | First byte is the size of string, follwed by data, example value translates to USB Audio                                                                                                                                                                                                                                                                                                                 |
|              | EEID (Monitor) |                                                   | EEID can be used to change the monitor manufacturer name and serial number, product type, capabilities, etc. EEID is in an arbitary position, search for the header `00 FF FF FF FF FF FF 00` followed by data, which is 256 bytes including the headers. More details on EEID data format can be found [here](https://en.wikipedia.org/wiki/Extended_Display_Identification_Data#EDID_1.4_data_format). |
| Last 4 bytes | Checksum       | `25 F7 72 6C`                                     | The checksum data comes right after code ends. First two bytes, `25 F7` is the checksum of bytes `02-0B` and `10-2F` and last two bytes, `72 6C` is the checksum of code.                                                                                                                                                                                                                                |

## Useful Tools

### [ms21xx-firmware](https://github.com/sandbox-pokhara/ms21xx-firmware)

Tool to generate ms21xx firmware with custom VID, PID, EDID, descriptors and serial number.

### [MS21XX MS91XX Download Tool](https://mega.nz/file/HfpAnIzB#UY7eqQpnL4wJM2C5Lne6Y_5GpIF37_AqLIG4hosE0sk)

This tool can be used to read/flash the firmware via HID interface.

1. Open File -> Load a firmware from file into the program (does not flash)
1. Connect -> Used to connect capture card if default VID, PID is changed
1. Download -> Flash the loaded firmware to the capture card
1. Read -> Read the firmware that is in your capture card (use this first time and make a backup of your original firmware)
1. Save to BIN -> Save the firmware to file

![MS21XX Download Tool](ms21xx_download_tool.png)

### [ms-tools](https://github.com/BertoldVdb/ms-tools)

Program, library and reference designs to develop for MacroSilicon MS2106/MS2109/MS2130 chips.

### [HxD](https://mh-nexus.de/en/hxd/)

HxD is a hex editor. It can be used to edit firmware for MS21XX.

### [EDID Decode](https://people.freedesktop.org/~imirkin/edid-decode/)

EDID Decode is a online web app to decode hex EDID values to readable format.

### [Monitor Asset Manager](https://www.entechtaiwan.com/util/moninfo.shtm)

Tool to parse EDID of your monitor.
