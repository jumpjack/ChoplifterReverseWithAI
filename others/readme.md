# Other games disassembled

## Resources

- [Online .d64 viewer and PRG extractor](https://d64.co/#)
- [Online disassembler](https://www.white-flame.com/wfdis/) 

## Side pacman

![image](https://github.com/user-attachments/assets/5ed7d80d-ed3f-41ec-8652-69cd2b2ebc0a)

### ROM

- [Version by mr.Z](https://csdb.dk/release/?id=184356)
- [Mirror](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/Pacman%20%5BMr.%20Z%5D.d64)

### Disk structure

![image](https://github.com/user-attachments/assets/56ecfc23-2f10-4a6a-8cb2-738613ecf38c)

### PRG file

- [](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/SIDE%20PACMAN.PRG)

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

Loader copies fragment 0x0835-0x0934 to 0x0100-0x01ff then jumps to 0x0100, which will contain this (relocated) code; here below is reported this code, in its original position and relocated:

```
Originale                       |   Rilocato a $0100
-------------------------------------------------------------------
0838: 78       L0838      SEI         |   0100: 78       L0100      SEI
0839: A9 34               LDA #$34    |   0101: A9 34               LDA #$34
083B: 85 01               STA $01     |   0103: 85 01               STA $01
083D: A0 00               LDY #$00    |   0105: A0 00               LDY #$00
083F: A9 F9               LDA #$F9    |   0107: A9 F9               LDA #$F9
0841: 85 AE               STA $AE     |   0109: 85 AE               STA $AE
0843: A9 68               LDA #$68    |   010B: A9 68               LDA #$68
0845: 85 AF               STA $AF     |   010D: 85 AF               STA $AF
0847: A9 00               LDA #$00    |   010F: A9 00               LDA #$00
0849: 85 AC               STA $AC     |   0111: 85 AC               STA $AC
084B: 85 AD               STA $AD     |   0113: 85 AD               STA $AD
084D: A5 AC    L084D      LDA $AC     |   0115: A5 AC    L0115      LDA $AC
084F: D0 02               BNE L0853   |   0117: D0 02               BNE L011B
0851: C6 AD               DEC $AD     |   0119: C6 AD               DEC $AD
0853: C6 AC    L0853      DEC $AC     |   011B: C6 AC    L011B      DEC $AC
0855: A5 AE               LDA $AE     |   011D: A5 AE               LDA $AE
0857: D0 02               BNE L085B   |   011F: D0 02               BNE L0123
0859: C6 AF               DEC $AF     |   0121: C6 AF               DEC $AF
085B: C6 AE    L085B      DEC $AE     |   0123: C6 AE    L0123      DEC $AE
085D: B1 AE               LDA ($AE),y |   0125: B1 AE               LDA ($AE),y
085F: 91 AC               STA ($AC),y |   0127: 91 AC               STA ($AC),y
0861: A5 AE               LDA $AE     |   0129: A5 AE               LDA $AE
0863: C9 BE               CMP #$BE    |   012B: C9 BE               CMP #$BE
0865: D0 E6               BNE L084D   |   012D: D0 E6               BNE L0115
0867: A5 AF               LDA $AF     |   012F: A5 AF               LDA $AF
0869: C9 08               CMP #$08    |   0131: C9 08               CMP #$08
086B: D0 E0               BNE L084D   |   0133: D0 E0               BNE L0115
086D: A9 01               LDA #$01    |   0135: A9 01               LDA #$01
086F: 85 AE               STA $AE     |   0137: 85 AE               STA $AE
0871: A9 08               LDA #$08    |   0139: A9 08               LDA #$08
0873: 85 AF               STA $AF     |   013B: 85 AF               STA $AF
0875: B1 AC    L0875      LDA ($AC),y |   013D: B1 AC    L013D      LDA ($AC),y
0877: C9 BF               CMP #$BF    |   013F: C9 BF               CMP #$BF
0879: D0 12               BNE L088D   |   0141: D0 12               BNE L0155
087B: 20 78 01            JSR $0178   |   0143: 20 78 01            JSR $0178
087E: B1 AC               LDA ($AC),y |   0146: B1 AC               LDA ($AC),y
0880: AA                  TAX         |   0148: AA                  TAX
0881: A9 00               LDA #$00    |   0149: A9 00               LDA #$00
0883: 91 AE    L0883      STA ($AE),y |   014B: 91 AE    L014B      STA ($AE),y
0885: 20 7F 01            JSR $017F   |   014D: 20 7F 01            JSR $017F
0888: CA                  DEX         |   0150: CA                  DEX
0889: D0 F8               BNE L0883   |   0151: D0 F8               BNE L014B
088B: F0 16               BEQ L08A3   |   0153: F0 16               BEQ L016B
088D: C9 CF    L088D      CMP #$CF    |   0155: C9 CF    L0155      CMP #$CF
088F: D0 0D               BNE L089E   |   0157: D0 0D               BNE L0166
0891: 20 78 01            JSR $0178   |   0159: 20 78 01            JSR $0178
0894: B1 AC               LDA ($AC),y |   015C: B1 AC               LDA ($AC),y
0896: AA                  TAX         |   015E: AA                  TAX
0897: 20 78 01            JSR $0178   |   015F: 20 78 01            JSR $0178
089A: B1 AC               LDA ($AC),y |   0162: B1 AC               LDA ($AC),y
089C: D0 E5               BNE L0883   |   0164: D0 E5               BNE L014B
089E: 91 AE    L089E      STA ($AE),y |   0166: 91 AE    L0166      STA ($AE),y
08A0: 20 7F 01            JSR $017F   |   0168: 20 7F 01            JSR $017F    ; ---> jump outside
08A3: 20 78 01 L08A3      JSR $0178   |   016B: 20 78 01 L016B      JSR $0178
08A6: D0 CD               BNE L0875   |   016E: D0 CD               BNE L013D
08A8: A9 37               LDA #$37    |   0170: A9 37               LDA #$37
08AA: 85 01               STA $01     |   0172: 85 01               STA $01
08AC: 58                  CLI         |   0174: 58                  CLI
08AD: 4C 0D 08            JMP L080D   |   0175: 4C 0D 01            JMP L010D
08B0: E6 AC               INC $AC     |   0178: E6 AC               INC $AC
08B2: D0 02               BNE L08B6   |   017A: D0 02               BNE L017E
08B4: E6 AD               INC $AD     |   017C: E6 AD               INC $AD
08B6: 60       L08B6      RTS         |   017E: 60       L017E      RTS
```

Only relocated, with AI-generated comments:

```
; Routine di decompressione/caricamento per Commodore 64

0100: 78       SEI          ; Disabilita gli interrupt
0101: A9 34    LDA #$34     ; Configura il banco di memoria:
0103: 85 01    STA $01      ; BASIC ROM off, I/O on, KERNAL ROM on

0105: A0 00    LDY #$00     ; Inizializza Y a 0 (usato come offset)
0107: A9 F9    LDA #$F9     ; Inizializza il puntatore sorgente
0109: 85 AE    STA $AE      ; a $68F9 (little-endian)
010B: A9 68    LDA #$68     ; 
010D: 85 AF    STA $AF      ; 

010F: A9 00    LDA #$00     ; Inizializza il puntatore destinazione
0111: 85 AC    STA $AC      ; a $0000 (little-endian)
0113: 85 AD    STA $AD      ; 

; Loop principale di copia/decompressione
0115: A5 AC    LDA $AC      ; 
0117: D0 02    BNE $011B    ; Decrementa il puntatore destinazione
0119: C6 AD    DEC $AD      ; (16-bit decrement)
011B: C6 AC    DEC $AC      ; 

011D: A5 AE    LDA $AE      ; 
011F: D0 02    BNE $0123    ; Decrementa il puntatore sorgente
0121: C6 AF    DEC $AF      ; (16-bit decrement)
0123: C6 AE    DEC $AE      ; 

0125: B1 AE    LDA ($AE),Y  ; Carica un byte dalla sorgente
0127: 91 AC    STA ($AC),Y  ; Salva il byte nella destinazione

0129: A5 AE    LDA $AE      ; Controlla se il puntatore sorgente
012B: C9 BE    CMP #$BE     ; ha raggiunto $08BE
012D: D0 E6    BNE $0115    ; Se no, continua il loop
012F: A5 AF    LDA $AF      ; 
0131: C9 08    CMP #$08     ; 
0133: D0 E0    BNE $0115    ; 

; Seconda fase: reinizializza i puntatori
0135: A9 01    LDA #$01     ; Reimposta il puntatore sorgente
0137: 85 AE    STA $AE      ; a $0801 (inizio area BASIC)
0139: A9 08    LDA #$08     ; 
013B: 85 AF    STA $AF      ; 

; Seconda loop principale (possibile decompressione o post-processing)
013D: B1 AC    LDA ($AC),Y  ; Carica un byte dalla destinazione
013F: C9 BF    CMP #$BF     ; Controlla se è $BF (marker speciale)
0141: D0 12    BNE $0155    ; Se non è $BF, passa al prossimo check

; Gestione marker $BF: riempimento con zeri
0143: 20 78 01 JSR $0178    ; Incrementa puntatore destinazione
0146: B1 AC    LDA ($AC),Y  ; Carica il conteggio
0148: AA       TAX          ; Trasferisce il conteggio in X
0149: A9 00    LDA #$00     ; Prepara 0 per il riempimento
014B: 91 AE    STA ($AE),Y  ; Scrive 0 nella nuova destinazione
014D: 20 7F 01 JSR $017F    ; Incrementa nuova destinazione
0150: CA       DEX          ; Decrementa contatore
0151: D0 F8    BNE $014B    ; Ripeti fino a fine conteggio
0153: F0 16    BEQ $016B    ; Salta alla fine del loop

; Controlla per il marker $CF
0155: C9 CF    CMP #$CF     ; Controlla se è $CF (altro marker speciale)
0157: D0 0D    BNE $0166    ; Se non è $CF, copia normalmente

; Gestione marker $CF: riempimento con valore specifico
0159: 20 78 01 JSR $0178    ; Incrementa puntatore destinazione
015C: B1 AC    LDA ($AC),Y  ; Carica il conteggio
015E: AA       TAX          ; Trasferisce il conteggio in X
015F: 20 78 01 JSR $0178    ; Incrementa puntatore destinazione
0162: B1 AC    LDA ($AC),Y  ; Carica il valore da ripetere
0164: D0 E5    BNE $014B    ; Vai al loop di riempimento

; Copia normale di un byte
0166: 91 AE    STA ($AE),Y  ; Copia il byte nella nuova destinazione
0168: 20 7F 01 JSR $017F    ; Incrementa nuova destinazione

; Fine del loop e controllo
016B: 20 78 01 JSR $0178    ; Incrementa puntatore destinazione
016E: D0 CD    BNE $013D    ; Se non zero, continua il loop

; Ripristino configurazione e fine
0170: A9 37    LDA #$37     ; Ripristina configurazione normale memoria:
0172: 85 01    STA $01      ; BASIC ROM on, I/O on, KERNAL ROM on
0174: 58       CLI          ; Riabilita gli interrupt
0175: 4C 0D 01 JMP $010D    ; Salta all'inizio (forse per un nuovo ciclo?)

; Subroutine: Incrementa puntatore destinazione
0178: E6 AC    INC $AC      ; Incrementa byte basso
017A: D0 02    BNE $017E    ; Se non c'è overflow, fine
017C: E6 AD    INC $AD      ; Se c'è overflow, incrementa byte alto
017E: 60       RTS          ; Ritorna dalla subroutine
 ```
Diagram:

[![image](https://github.com/user-attachments/assets/5bba6468-35a3-48a8-85b1-ae7cf8dfa5d5)](https://github.com/user-attachments/assets/4ff36226-6342-4a0b-aff7-4c113fb6d908)

On [draw.io](https://drive.google.com/file/d/1Q6d6qzGcbXWV57MXessnncWx0gNYwzgh/view?usp=sharing).

## Attack of mutant camels

![image](https://github.com/user-attachments/assets/e63dd68d-8289-44fe-b6a6-cb78b3b42294)

Disassembly for C64 (external site): [link](https://github.com/C64-Mark/Attack-of-the-Mutant-Camels)

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




