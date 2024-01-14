Moonblaster 1.4

Moonblaster comes on a single sided disk.

The copy Proteection:

On Track 1 (Side0), Sector 6 is not formatted as a standard 512 bytes long sector, instead it is 1025 bytes.
This sector contains following 512 bytes of tekst, repeated once more.

Code:
MOONBLASTER V1.3   Deze diskette is een product van het MOONSOFT TEAM in samenwerking met het MAGIC TEAM. De diskette is ten alle tijden tegen betaling te verkrijgen bij STICHTING SUNRISE. Aan dit programma is 1.5 jaar gewerkt en mocht u besluiten om deze diskette te kopieren of te kraken dan moet u ook beseffen dat u dan de MSX markt ten gronde richt, omdat wij dan ook geen zin meer hebben om nieuwe software uit te brengen, (en er komt toch al zo weinig uit!!!).��������������������������������������������� 


When this disk is copied, sector 6 will be divided into 2 (512 bytes) sectors on the destination disk.
This creates an extra sector, which means that all subsequent sectors are shifted up one sector

This will cause the copied disk fail to boot, the msx will keep restarting continuously.




How to defeat the copy protection and create a normal .DSK file:

Moonblaster is loaded by reading several sectors from disk to memory.


File	LD in mem	Nr of sec	Begin sec	Datablock search	Note
01		0xC200		03			0x000F		11 14 00 21 00 90	
02		0x9000		11			0x0014		92 b4 00 01 09 80		Sunrise Logo
03		0xB000		01			0x000A		11 0d 00 06 01 21	
04		0xA000		01			0x000D		7D 9B FD 4D 70 5D		Xor 0x5C
05		0xA1C7		01			0x012D		07 77 00 00 06 70	
06		0x8000		04			0x0131		FC 84 84 84 84 84	
07		0xD800		01			0x0118		83 DD 96 6F 4E CE		Heavily encoded with several Xor values
08		0x8000		01			0x000E		4D 4F 4F 4E 42 4C	
09		0x8000		01			0x015D		02 00 40 40 40 40	
10		0x8000		31			0x0040		E5 2A DC F3 E5 87	
11		0x8000		31			0x0060		E5 2A DC F3 E5 87	
12		0x8000		31			0x0080		
13		0x8000		02			0x00A0		01 02 03 04 05 06	
14		0x8000		10			0x00C0		42 61 73 73 20 31	
15		0x8000		02			0x00E0		20 20 30 20 20 31	
16		0x8000		01			0x0100		F5 DB FD 32 3C D9	
17		0x8000		20			0x0020		


Because sector 6 is now divided into 2 separate sectors,
it causes a shift in the following sectors on the disk.

As result, the sector numbers in the loading software do not longer match.

That's why we first have to move sectors up by one sector (starting at sector 7) to get the files back in the right place,
this results in the 2nd part (512 byte) of sector 6 being overwritten


If we start from the copied disk after shifting up the sectors, 
the loading process will progress a little further than last time.
we now even see the Sunrise logo appear on screen.


Unfortunately loading will fail again and all kinds of garbage will be written on screen followed by a reboot.

This happens because a 'second copy protection check' is built-in to the loader to prevent copied disks from working.
This routine loads the manipulated 1024 Bytes sector 6 into memory at address 0x8000, and checks whether 
the first 512 bytes and the 2nd 512 bytes of this sector are still the same. 
(which it is not the case, because this sector is now split and we have written over the 2nd sector when we shifted all sectors)

This 'second copy protection check' routine is stored on disk heavily encrypted with several iteration
of Xor operations. So we are not going to mess with that.
Instead we wait until this routine is loaded in memory unencrypted at adress 0xD800 
and intercept it when this routine is called.
We only need to change one Byte to disable the 'second copy protection check' (Writing a 0xC9 [RET] to adress 0xD8B7)


- Security is only as strong as its weakest link

The code that calls the 'second copy protection check' routine is located in address 0xA000 
and has already been loaded into memory at this point.

I found out that this part is stored on disk with a very simple (Xor 0x5C) encryption.
So it was very easy to find and edit the (jp 0xD800) jump adress.
By changing bytes 5C,84 at adress 0x1B82 into 98,9C, we now have redirected the jump to adress 0xC0C4
(0xC0C4 points to free memory we can use to disable the 'second copy protection check'
it will be loaded from the bootsector on the disk. 

















