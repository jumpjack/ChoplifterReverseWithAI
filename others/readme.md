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

