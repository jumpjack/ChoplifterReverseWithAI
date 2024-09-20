# Other games disassembled

## Side pacman

![image](https://github.com/user-attachments/assets/5ed7d80d-ed3f-41ec-8652-69cd2b2ebc0a)


### Basic loader

`2204 SYS 2087 SHORT VERSION BY MR.Z    `

### Assembly loader (starting from 2087dec, 0x0827)

![image](https://github.com/user-attachments/assets/a906b0ea-f22c-4a7d-8d80-c3ece6d94591)
```
0827: A2 00     LDX #$00    ; Carica il valore 0 nel registro X
0829: BD 38 08  LDA $0838,X ; Carica un byte dalla memoria all'indirizzo $0838+X in A
082C: 9D 00 01  STA $0100,x
082F: E8        INX         ; Incrementa X
0830: D0 F7     BNE $0829   ; Salta indietro a $0829 se X non è zero (cioè ripete per 255 valori)
0832: A2 FF     LDX #$FF    ; Carica il valore 255 nel registro X
0834: 9A        TXS         ; Trasferisce X nello stack pointer
0835: 4C 00 01  JMP $0100   ; Salta all'indirizzo $0100
```

Loader copies fragment 0x0835-0x0934 to 0x0100-0x01ff then jumps to 0x0100, which will contain this (relocated) code:

```
0100: 78        SEI
0101: A9 34     LDA #$34
0103: 85 01     STA $01
0105: A0 00     LDY #$00
0107: A9 F9     LDA #$F9
0109: 85 AE     STA $AE
010B: A9 68     LDA #$68
010D: 85 AF     STA $AF
010F: A9 00     LDA #$00
0111: 85 AC     STA $AC
0113: A5 AC     LDA $AC
0115: D0 02     BNE $0119
0117: C6 AD     DEC $AD
0119: C6 AC     DEC $AC
011B: A5 AE     LDA $AE
011D: D0 02     BNE $0121
...
...
```


## Attack of mutant camels

![image](https://github.com/user-attachments/assets/e63dd68d-8289-44fe-b6a6-cb78b3b42294)

### BASIC loader

10 SYS 4096

### Assembly loader ($1000, dec4096)

![image](https://github.com/user-attachments/assets/40c8ee28-4ac0-43df-9807-e4da79de0b9c)

****

![image](https://github.com/user-attachments/assets/a7ae5a62-cc5a-48bd-b729-0b565d5b4323)

****

![image](https://github.com/user-attachments/assets/d18a4bf0-0078-4307-9a85-dfc9202763ad)

(dozens of lda/sta, i.e. variables initialization)

then:

![image](https://github.com/user-attachments/assets/8044f6ff-aa0b-4dca-81f5-2d080e59ad8b)

****

![image](https://github.com/user-attachments/assets/fe7b3bad-e0f0-4445-8f19-8f41c92328c5)

(other LDA/STA until...)

![image](https://github.com/user-attachments/assets/aba3e373-b490-4167-951c-c9964d3ce929)

****

![image](https://github.com/user-attachments/assets/9bc2c5a2-31c6-4a66-b34b-01af518b03ac)

(dozens of nop until...)

![image](https://github.com/user-attachments/assets/66f0e94f-f486-4d61-9f41-ce30a10d656f)

So at $1500 we have main game loop.




