# The Last Dimension,  

[The Last Dimension (Compjoetania)[DMK].zip](https://download.file-hunter.com/Music/Music%20Discs/The%20Last%20Dimension%20(Compjoetania)[DMK].zip)  
[The Last Dimension (Compjoetania).zip](https://download.file-hunter.com/Music/Music%20Discs/The%20Last%20Dimension%20(Compjoetania).zip)  
<br>

## The Copy Protection:

This game comes on a double sided_disk.

On this disk, Track 1, side 0 has a special format. It has only 2 sectors instead of the usual 9 sectors.  
```
-- physical track 1, head 0  
 0: AOfst= 135 C=  1 H=  0 R=  1 N=255 ACrc=8269,ok  DOfst= 179 T=n DCrc=2121,ERR
 1: AOfst= 964 C=  1 H=  0 R=246 N=  2 ACrc=368d,ok  DOfst=1008 T=n DCrc=b63b,ok 
```

The data on sector 246 is read using a trick involving a hook at address `#F27F` called by the DiskROM.  
This hook calculates the **physical sectors** and tack number out of a **logical sector** number. `(BC=BC/DE; HL=rest routine)`  
In this case `BC=0x2f5` & `DE=0x100`, `0x2f5/0x100` = `2 /2 sides = Track 1`, Rest `#F5`.  
This remainder `0xf5` determines the sector number.  
Since a physical sector starts at **1** (and not **0**), we add `1`, this results in `0xf6` = Sector `246`.  

If this disk is copied using a standard sector-copying program, the special sector 246 will no longer exist on track 1,  
this is why the demo on the copied disk fails to load any further. 
Neither Fastcopy 3.0 nor Allcopy can copy this disk, both programs return a error message.  


![Sector overview.](https://github.com/LarsThe18Th/MSX_Copyprotection/blob/main/The_Last_Dimension/Image1.jpg) 
![Sector detail.](https://github.com/LarsThe18Th/MSX_Copyprotection/blob/main/The_Last_Dimension/Image2.jpg)  
<br>

## How to defeat the copy protection and create a normal .DSK file: 


First copy all sectors except the special sector 246 on track 1 side 0, to another disk. (Track 1 side 1 can be copied normally).  

We need to move the 512 bytes of data in the special sector 246 to another location.  
As an example, we will use `Sector 107` `track 14` `sector 3` (@00020E00 on the DSKimage),  
which is also used in [Eugeny_Brychkov](https://www.msx.org/forum/msx-talk/software/bugs-in-and-solution-for-the-last-dimension-by-the-compjoetania?page=0
) explanation (though any other free sector would work too).  
In this case `BC=0x107` & `DE=0x009`, `0x107/0x009` = `0x1d (29) /2 sides = Track 14`, Rest `2(+1) = Sector 3`.  

Now we just need to adjust the loader to the new location of the special sector data using a hex editor.  
Search for Hex values `07 01 F5 02 11 00 01 C9` and replace it by `07 01 07 01 11 09 00 C9`
 


## Note:
If all this was too difficult, 
- I have written a sample program that allows you to copy the original **The Last Dimension** to another disk without copy protection.   
You will need an MSX with two disk drives to do this.
- The original disk contains an extra track on side 0 (Track 80) with extra information that is not important for copy protection.  
After an `XOR 0xFF` operation in a HexEditor we see the following information.  
`DD-001A-04  09/30/93 17:39__5101 3850-8_____000210__________`



