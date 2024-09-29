# Analyzing  variants of "Attack of the mutant camels"

## 001

### ROM

Advance of The Mega Camels - AMC (Llamasoft)[cr wcs].d64

### Disk contents

![image](https://github.com/user-attachments/assets/9fc5054d-bbea-40bd-837e-3b442239a575)

### Main BASIC program

- [PRG file](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/camels/PRG/A.M.C..PRG)

As seen on screen (VICE):

![image](https://github.com/user-attachments/assets/c0346881-baec-4522-a836-cd2880e88d67)

Actual contents:

![image](https://github.com/user-attachments/assets/0742af06-7d66-4650-9176-cb2546b737d0)

(calling $0819/dec2213)

Hex dump:

```
000000: 01 08 17 08 c1 07 00 57 43 53 20 9e 32 30 37 33  ....A..wcs .2073
000010: 20 20 20 57 43 53 20 00 00 00 a9 36 85 01 4c 9c     wcs ....6..l.
000020: 30 08 14 00 93 22 43 41 4d 45 4c 4f 42 4a 22 2c  0...."camelobj",
000030: 38 2c 31 00 03 d3 35 33 32 34 38 aa 32 34 2c 32  8,1..S53248.24,2
000040: 32 3a 99 c7 28 38 29 00 53 08 c8 00 93 22 22 2c  2:.G(8).s.H.."",
000050: 31 2c 31 00 03 d3 ff 00 00 ff ff 00 00 ff ff 00  1,1..S..........
```

Dump analysis
```
01 08: Program start address = $0801

$0801   17 08              end of line = $0817
$0803   c1 07              line number = $07c1 = dec1985
$0805   00                 trick to hide program code?
$0806   57 43 53 20       "WCS "
$080a   9e                "SYS" token
$080b   32 30 37 33 20    "2073 " ($0819)
$0810   20 20 57 43 53 20 "  WCS "
$0816   00
$0817   00
$0818   00
$0819   a9 36             lda   #$36            
$081b   85 01             sta   $01             
$081d   4c 9c 30          jmp   $309C           

``` 

## 002 

### ROM

attackfuckincamels-Pirate-Soft-sb056449-Player1Part1.d64

### Main BASIC program

Extracted with https://www.c64-tools.com/basic-2-extractor

![image](https://github.com/user-attachments/assets/e0bb7226-38d2-4c51-9137-2b635606f970)

- Main lop: $1500-$1560?
- Charset from $2000
- Sprites from $3000


## 003

###
--

