# Radarsoft Games,  
### In this case, Eindeloos and Kruiswoord Puzzel   
[Eindeloos (1986)(Radarsoft)(nl).zip](https://download.file-hunter.com/Games/DMK-Files/Eindeloos%20(1986)(Radarsoft)(nl).zip)  
[Kruiswoord Generator (1986)(Radarsoft)(nl).zip](https://download.file-hunter.com/Games/DMK-Files/Kruiswoord%20Generator%20(1986)(Radarsoft)(nl).zip)  
<br>

## The Copy Protection:

These games come on a single sided disk.  

Track 77, Sector 1, is preformatted with `F5,E5,E5,E5,E5,E5,E5,E5...`  
The copy protection check will load Track 77, Sector 1 in memory at address `0xD400` and then do a test on the first byte.  
If it is `F5` the loading will proceed.  
If it is *NOT* `F5` then the MSX will be beeping continuously.

Odd thing is that tracks 78 and 79 are unformatted... not sure why.  
The copy protection only works if the individual files are copied to another floppy disk,  
copying the entire disk using a simple sector copier does not trigger the copy protection.  
This is also the reason why these games do not work from a hard drive.

![Sector overview.](https://github.com/LarsThe18Th/MSX_Copyprotection/blob/main/Radarsoft/Image1.jpg) 
![Sector detail.](https://github.com/LarsThe18Th/MSX_Copyprotection/blob/main/Radarsoft/Image2.jpg)  
<br>

## How to defeat the copy protection and create a normal .DSK file: 

We found out that Track 77, Sector 1 is loaded in memory at address `0xD400`
and looked up the assembly code that takes care of this.


Asm:  
```
LD	A,(#D400)	3A 00 D4 
CP	#F5		FE F5  
JP	Z,#C025		CA 25 C0  
CALL	#00C0		CD C0 00  
```

To disable the copy protection, we need to change the conditional jump instruction into a normal jump. 

To change this on the DSK file, search for the following HEX values with a HexEditor,  
`CA 25 C0 CD C0 00`

And change it to,  
`C4 25 C0 CD C0 00`


In my case, on address 0x14A23 of the DSK file.  
Now the individual files can be copied to another floppy disk without triggering the copy protection.  
<br>

## Note:
If all this was too difficult, 
- This disk can be copied using ANY sector-copying program.
