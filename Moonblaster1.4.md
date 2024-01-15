# Moonblaster 1.4

## The copy Protection:

Moonblaster comes on a single sided disk.  
On Track 1 (Side0) of the disk, we see that the size of Sector 6 is modified, it is 1024 bytes in stead of the standard 512 Bytes.  

In this 1024 Bytes sector we see the following text written twice.  

> MOONBLASTER V1.3   Deze diskette is een product van het MOONSOFT TEAM in samenwerking met het MAGIC TEAM. De diskette is ten alle tijden tegen betaling te verkrijgen bij STICHTING SUNRISE. Aan dit programma is 1.5 jaar gewerkt en mocht u besluiten om deze diskette te kopieren of te kraken dan moet u ook beseffen dat u dan de MSX markt ten gronde richt, omdat wij dan ook geen zin meer hebben om nieuwe software uit te brengen, (en er komt toch al zo weinig uit!!!).���������������������������������������������  

If this disk is copied with a standard sector copier, sector 6 will be split into 2 seperate (512 bytes) sectors on the destination disk.  
This adds an extra sector to the disk, which means that all subsequent sectors are shifted down one place.  
As a result the copied disk will not be able to boot, the msx will keep restarting continuously.


## How to defeat the copy protection and create a normal .DSK file:

Moonblaster is loaded by reading several sectors from disk in to memory.

|File|LD in mem|Nr of sec|Begin sec|Datablock search|Notes|
| :------------ | :------------ | :------------ | :------------ | :------------ | :------------ |
|01|0xC200|03|0x000F|11 14 00 21 00 90|Loader
|02|	0x9000|11|0x0014|92 b4 00 01 09 80|Sunrise Logo|
|03|0xB000|01|0x000A|11 0d 00 06 01 21||
|04|0xA000|01|0x000D|7D 9B FD 4D 70 5D|Xor 0x5C|
|05|0xA1C7|01|0x012D|07 77 00 00 06 70||
|06|0x8000|04|0x0131|FC 84 84 84 84 84||
|07|0xD800|01|0x0118|83 DD 96 6F 4E CE|Heavily encrypted with several Xor values|
|08|0x8000|01|0x000E|4D 4F 4F 4E 42 4C||
|09|0x8000|01|0x015D|02 00 40 40 40 40||
|10|0x8000|31|0x0040|E5 2A DC F3 E5 87||
|11|0x8000|31|0x0060|E5 2A DC F3 E5 87||
|12|0x8000|31|0x0080|||
|13|0x8000|02|0x00A0|01 02 03 04 05 06||
|14|0x8000|10|0x00C0|42 61 73 73 20 31||
|15|0x8000|02|0x00E0|20 20 30 20 20 31||
|16|0x8000|01|0x0100|F5 DB FD 32 3C D9||
|17|0x8000|20|0x0020|||


As a result of the sector shift, the sector numbers used by the loader software no longer match up.  
To counter this, all sectors must move up one place from (track 1 side 0) sector 7 onwards,  
this results in the 2nd part of the split sector 6 (Now in sector 7) will be overwritten.  


If we try to boot from the copied disk after shifting up the sectors, the loading process will progress a little further than last time.  
We now even see the Sunrise logo appear on screen.  
Unfortunately loading will fail again and all kinds of garbage will be written on screen, followed by a reboot.  


## The copy protection check
The garbled screen and reboot happens because a 'copy protection check' is built-in to the loader to prevent a copied disks from working.  
This routine loads the manipulated 1024 Bytes of sector 6 into memory at address 0x8000,  
and checks whether the first 512 bytes and the 2nd 512 bytes of this sector are still the same.  
(which it is not the case, because this sector is now split and we have written over the 2nd part when we shifted all sectors)  


This 'copy protection check' routine is stored on disk with very heavy encryption(a few iteration of Xor operations),so we are not going to mess with that.  
Instead we will wait until this routine is loaded in memory unencrypted (at adress 0xD800),  
and intercept it when this routine is called.  
I found out we only need to change one Byte to disable the 'copy protection check' (Writing a 0xC9 [RET] to adress 0xD8B7)  


## Security is only as strong as its weakest link

The code that calls the 'copy protection check' routine is loded from address 0xA000 and has already been loaded into memory at this point.  

I found out that this part is stored on disk with a very simple (Xor 0x5C) encryption.  
It is now very easy to find the (jp 0xD800) jump adress and change it.  
By changing bytes 5C,84 at adress 0x1B82 into 98,9C, we now have redirected the jump to adress 0xC0C4.  
This adres points to free memory where the bootsector of the disk is stored.  
This gives us the opportunity to add code that disables the 'copy protection check' and then start it.  
We add the following code ld a,#c9 / ld (d8b7),a / jp #d800 (E3 C9 32 B7 D8 C3 00D8)  


After all these changes the copied disk should start normaly and launch Moonblaster 1.4.  


## Note:
If all this is too difficult, 
- You can copy the original Moonblaster 1.4 disk with 'AllCopy' (Special-Optie enabled).  
- Or you can download a Public Domain version for free, with a manual and sourcecode included.  