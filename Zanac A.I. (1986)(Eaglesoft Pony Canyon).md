# Zanac A.I. (1986)(Eaglesoft Pony Canyon).DMK image.  
https://download.file-hunter.comGamesDMK-FilesZanac%20A.I.%20(1986)(Eaglesoft%20Pony%20Canyon).zip  

## The copy Protection:

Zanac comes on a single sided disk.  

Only the first 40 tracks of side 0 are formatted.  
The copy protection check tries to read beyond sector 40 to cause a error.  

If a error occurs (Because sector is not readable) the diskrom jumps to the error pointer  
in adress 0xF323 and from that adress the game continues loading.  

When this disk is copied with a sector copier, the read error will not be triggered.  
(Because all sectors beyond 40 ARE now formatted and readable)  
The loader will return to Screen 0 and write 0xC0 192 times to memory from adress 0x0141 to clean the loader from memory.  
Finaly it jumps back to Basic  

## How to defeat the copy protection and create a normal .DSK file: 
Set breakpoint @029C

Read System variable F323 to see pointer to what adress will be jumped when a error occurs.

(0x039b) @ this adress you find e1 and 02 = 0x02e1


In the DSK file:


Search for CD 9D 03 C3 53 01 C5

@adress 199c change CD 9D 03   to CD E1 02


