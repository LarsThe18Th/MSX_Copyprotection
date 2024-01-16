# MSX Copy protections  

\
**Cheatsheet with useful addresses:**  


#### Diskrom Functions 0xF37D  

- 0x0F  
Pointer to FCB (Filename) in memory in Register DE
- 0x1A  
Pointer to destination adress in memory Register DE
- 0x27  
Blockread read file in memory (As in 0xF0 & 0x1A)  
DE = Pointer to FSB (like in 0x0F)  
HL = Number of Records  
Returns:  
A = 0 = No Error / 1= Error
HL = Number of sectors actually read

- 0x2F  
Absolute sector read  
DE = Sector number (Adress on DSK -> DE = 02c8 x 512 Bytes(0x200) = 0x59000.  
L = DriveNr (0 = A drive) ( 1 = B drive )  
H = Number of sectors to read  
Returns:  
A = (0 = No Error) (1= Error)  

- 0x10  
Close opened file 
Retuns:  
A or L = (00 = Ok) (FF = Error)  


 #### Sytem Variabels:  
- 0xF323 Error pointer address, jumps to this adress when a error happends  

#### Bootsector:
- 0x15

  | ID | Sides | Tracks | Sectors |
  | :------------: | :------------: | :------------: | :------------: |
  | F8 | Single | 80 | 9 |
  | F9 | Double | 80 | 9 |
  | FC | Single | 40 | 9 |
  | FD | Double | 40 | 9 |

- 0x1A

  | Val | Sides | Read Heads|
  | :------------: | :------------: | :------------: |
  | 01 | Single | 1 |
  | 02 | Double | 2 |


#### Disk Calculation:
- 2 Sides x 9 Sectors x 80 Tracks x 512 Bytes = 720KB

#### Links:

- Disk Cursus MCCM  
https://www.msxcomputermagazine.nl/mccm/millennium/milc/gc/topic_32.htm

- Topic about copyprotection on MSX.org  
https://www.msx.org/forum/msx-talk/software-and-gaming/about-copy-protections
