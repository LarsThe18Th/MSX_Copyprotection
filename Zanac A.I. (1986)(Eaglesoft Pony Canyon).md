Zanac A.I. (1986)(Eaglesoft Pony Canyon).DMK image.

https://download.file-hunter.comGamesDMK-FilesZanac%20A.I.%20(1986)(Eaglesoft%20Pony%20Canyon).zip

Description:

Only 40 sectors of side 0 is formatted,


The copyprotection reads beyond sector 40 to cause a error

If a error occurs (Because sector is not readable) the diskrom jumps to the error pointer in adress 0xF323


When the disk is copied the error will not be triggered (Because the sector is readable)
after the read is done it jumps to a destructive routine

Returns to Screen 0 and write garbage 0xC0's 192 time to adress 0x0141 (loading routine)
and finaly jump back to Basic




Set breakpoint @029C


Read System variable F323 to see pointer to what adress will be jumped when a error occurs.

(0x039b) @ this adress you find e1 and 02 = 0x02e1


In the DSK file:


Search for CD 9D 03 C3 53 01 C5

@adress 199c change CD 9D 03   to CD E1 02


