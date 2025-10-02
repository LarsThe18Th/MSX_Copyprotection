# De Erfenis - Paniek in Las Vegas (1987)(Infogrames)(NL)

## The copy Protection:

De Erfenis is distributed on a single-sided disk.
Track 79 of this disk appears to be empty (filled with the value 0xE5),  
but in reality the pre-gap areas of all 9 sectors on track 79 are padded with 0xF7 instead of the default 0x4E.

## The copy protection check

The loader uses the TMS279X disk controller's "Load Track" command to read track 79 completely into memory.  
This includes headers, data, CRC and GAP information. (about 2304 bytes)  
(Due to variations in rotation speed, it is possible that not all gap bytes are read on every attempt.)

The loader then counts the number of occurrences of the value 0xF7 in the loaded data.  
If 0xF7 appears more than 20 times, the protection check passes and the loader continues starting the game.  
If 0xF7 appears 0 times (or fewer than 21 times) after reading 2304 bytes, the protection check fails and the loader retries repeatedly.  

When the disk is copied at the sector level, the gap bytes on track 79 are reset to the default value 0x4E, which causes the protection to fail.

## How to defeat the copy protection and create a normal .DSK file:

Fill the last 2 sectors of the disk with Hex value 0xF7 so the loader finds enough 0xF7 bytes during its check.

or  

Search on the disk(Image) for the folowing Hex string 20 E5 C9 C5 21 00 80 11 and replace the first 2 bytes (20, E5) with 00, 00  
to disable the copy protection completely.



## Note:

- Because the loader accesses the disk controller directly, the game will only run with a TMS279X-compatible disk controller.  
  In other words, it will not run on MSX2+ or Turbo-R systems unless the game has been cracked.
- The game will also not run correctly if copied with AllCopy.