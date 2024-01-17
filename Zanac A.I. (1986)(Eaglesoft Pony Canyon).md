# Zanac A.I. (1986)(Eaglesoft Pony Canyon).DMK image.  
[Zanac A.I. (1986)(Eaglesoft Pony Canyon).zip](https://download.file-hunter.comGamesDMK-FilesZanac%20A.I.%20(1986)(Eaglesoft%20Pony%20Canyon).zip)  

## The copy Protection:

Zanac comes on a single sided disk.  

Only the first 10 tracks of side 0 are formatted, the copy protection check  tries to read sector 79 to cause a error.  

If an error occurs because a sector is not readable, the diskrom jumps to the error pointer  
in adress 0xf323 and from that adress the game continues loading.  

When this disk is copied with a sector copier, the read error will not be triggered.  
This happens because all sectors beyond 10 are now formatted and readable.  
After this the loader will return to Screen 0 and clears itself from memory by writing 0xC0 192 times from adress 0x0141.  
Finaly it jumps back to Basic.  

## How to defeat the copy protection and create a normal .DSK file: 

We first need to find out to what adress the loader jumps to if an read error happens on the disk.  
Looking at adress 0xf323 we see the pointer adress 0x039b, this pointer adress contains 0x02e1.  

I found out that we can intercept the the call on  memory adress 0x029C, that reads track 79.  
If we change this call to adress 0x02e1, the loader continues without the copy protection check.  

To change this on the DSK file, search for the following HEX values with a HexEditor,  
`CD 9D 03 C3 53 01 C5`

And change it to,  
`CD E1 02 C3 53 01 C5`


Im my case on adress 0x199c of the DSK file.  

## Note:
If all this was too difficult, 
- You can copy the original Zanac A.I. disk with `AllCopy` (Special-Optie enabled).  
