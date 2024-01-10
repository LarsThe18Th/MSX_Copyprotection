Zanac A.I. (1986)(Eaglesoft Pony Canyon).DMK image.
httpsdownload.file-hunter.comGamesDMK-FilesZanac%20A.I.%20(1986)(Eaglesoft%20Pony%20Canyon).zip

Description:
Only 40 sectors of side 0 is formatted, copyprotection reads beyond sector 40 to cause a error
and jumps to error pointer in adress 0xF323



Set breakpoint @029C

Read System variable F323 to see pointer to what adress will be jumped when a error occurs.
(0x039b) @ this adress you find e1 and 02 = 0x02e1

