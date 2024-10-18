# Other games disassembled

## How to

- Download a .d64 ROM image from [somewhere](https://csdb.dk/release/?id=184356)
- Extract PRG file from .d64 disk image with this [online .d64 viewer and PRG extractor](https://d64.co/#)
- Upload the PRG file to this [online disassembler](https://www.white-flame.com/wfdis/) 

## Side pacman

![image](https://github.com/user-attachments/assets/5ed7d80d-ed3f-41ec-8652-69cd2b2ebc0a)

### ROM

- [Version by mr.Z](https://csdb.dk/release/?id=184356)
- [Mirror](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/Pacman%20%5BMr.%20Z%5D.d64)

### Disk structure

![image](https://github.com/user-attachments/assets/56ecfc23-2f10-4a6a-8cb2-738613ecf38c)

### PRG file

- [](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/SIDE%20PACMAN.PRG)

It gets loaded from $0801 (BASIC area) to $68f9

### Basic loader

`2204 SYS 2087 SHORT VERSION BY MR.Z    `

dec 2087 is $0827

### Assembly loader (starting from 2087dec, 0x0827, finishing at 0x0837)

![image](https://github.com/user-attachments/assets/a906b0ea-f22c-4a7d-8d80-c3ece6d94591)

```
0827: A2 00     LDX #$00    ; Carica il valore 0 nel registro X
0829: BD 38 08  LDA $0838,X ; Carica un byte dalla memoria all'indirizzo $0838+X in A
082C: 9D 00 01  STA $0100,x
082F: E8        INX         ; Incrementa X
0830: D0 F7     BNE $0829   ; Salta indietro a $0829 se X non è zero (cioè ripete per 256 valori)
0832: A2 FF     LDX #$FF    ; Carica il valore 255 nel registro X
0834: 9A        TXS         ; Trasferisce X nello stack pointer
0835: 4C 00 01  JMP $0100   ; Salta all'indirizzo $0100
```

Loader copies next 256 bytes (0x0838-0x0937) to 0x0100-0x01ff then jumps to 0x0100, which will contain this (relocated) code; here below is reported this code, in its original position and relocated:

```
Originale                             |   Rilocato a $0100
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
      
08B7: E6 AE               INC aAE     |  017F: E6 AE               INC aAE     
08B9: D0 02               BNE b08BD   |  0181: D0 02               BNE b0183
08BB: E6 AF               INC aAF     |  0183: E6 AF               INC aAF
08BD: 60          b08BD   RTS         |  0185: 60          b0185   RTS 

08BE: 0B 08               ANC #$08    |  0186: 0B 08               ANC #$08
08C0: 0A                  ASL         |  0188: 0A                  ASL 
08C1: 00 9E               BRK #$9E    |  0189: 00 9E               BRK #$9E
08C3: 32                  JAM         |  018B: 32                  JAM 
08C4: 30 36               BMI b08FC   |  018C: 30 36               BMI b01C4
08C6: 31 BF               AND (pBF),Y |  018E: 31 BF               AND (pBF),Y
08C8: 03 A9               SLO (pA9,X) |  0190: 03 A9               SLO (pA9,X)
08CA: 00 8D               BRK #$8D    |  0192: 00 8D               BRK #$8D
08CC: 20 D0 8D            JSR e8DD0   |  0194: 20 D0 01            JSR e01D0
08CF: 21 D0       f08CF   AND (pD0,X) |  0197: 21 D0       f0197   AND (pD0,X)
08D1: 20 60 09            JSR s0960   |  0199: 20 60 01            JSR s0160
08D4: AD 0E DC            LDA $DC0E   |  019C: AD 0E DC            LDA $DC0E   
08D7: 29 FE               AND #$FE    |  019F: 29 FE               AND #$FE
08D9: 8D 0E DC            STA $DC0E   |  01A1: 8D 0E DC            STA $DC0E   
08DC: 78                  SEI         |  01A4: 78                  SEI 
08DD: A5 01               LDA a01     |  01A5: A5 01               LDA a01
08DF: 29 FC               AND #$FC    |  01A7: 29 FC               AND #$FC
08E1: 09 01               ORA #$01    |  01A9: 09 01               ORA #$01
08E3: 85 01               STA a01     |  01AB: 85 01               STA a01
08E5: A9 F6               LDA #<p0AF6 |  01AD: A9 F6               LDA #<p01F6
08E7: A0 0A               LDY #>p0AF6 |  01AF: A0 0A               LDY #>p01F6
08E9: 8D FE FF            STA aFFFE   |  01B1: 8D FE FF            STA aFFFE   
08EC: 8C FF FF            STY aFFFF   |  01B4: 8C FF FF            STY aFFFF   
08EF: A9 AD               LDA #<p10AD |  01B7: A9 AD               LDA #<p11AD
08F1: A0 10               LDY #>p10AD |  01B9: A0 10               LDY #>p11AD
08F3: 8D FC FF            STA aFFFC   |  01BB: 8D FC FF            STA aFFFC   
08F6: 8C FD FF            STY aFFFD   |  01BE: 8C FD FF            STY aFFFD   
08F9: 8D FA FF            STA aFFFA   |  01C1: 8D FA FF            STA aFFFA   
08FC: 8C FB FF    b08FC   STY aFFFB   |  01C4: 8C FB FF    b01C4   STY aFFFB   
08FF: A9 00               LDA #$00    |  01C7: A9 00               LDA #$00
0901: A2 B2               LDX #$B2    |  01C9: A2 B2               LDX #$B2
0903: 95 00       b0903   STA f00,X   |  01CB: 95 00       b01CB   STA f00,X
0905: CA                  DEX         |  01CD: CA                  DEX 
0906: E0 02               CPX #$02    |  01CE: E0 02               CPX #$02
0908: D0 F9               BNE b0903   |  01D0: D0 F9               BNE b01CB
090A: 8D 15 D0            STA $D015   |  01D2: 8D 15 D0            STA $D015   
090D: 8D 02 DC            STA $DC02   |  01D5: 8D 02 DC            STA $DC02   
0910: 8D 03 DC            STA $DC03   |  01D8: 8D 03 DC            STA $DC03   
0913: AD 02 DD            LDA $DD02   |  01DB: AD 02 DD            LDA $DD02   
0916: 09 03               ORA #$03    |  01DE: 09 03               ORA #$03
0918: 8D 02 DD            STA $DD02   |  01E0: 8D 02 DD            STA $DD02   
091B: AD 00 DD            LDA $DD00   |  01E3: AD 00 DD            LDA $DD00   
091E: 29 FC               AND #$FC    |  01E6: 29 FC               AND #$FC
0920: 09 02               ORA #$02    |  01E8: 09 02               ORA #$02
0922: 8D 00 DD            STA $DD00   |  01EA: 8D 00 DD            STA $DD00   
0925: A9 70               LDA #$70    |  01ED: A9 70               LDA #$70
0927: 09 08               ORA #$08    |  01EF: 09 08               ORA #$08
0929: 8D 18 D0            STA $D018   |  01F1: 8D 18 D0            STA $D018   
092C: AD 11 D0            LDA $D011   |  01F4: AD 11 D0            LDA $D011   
092F: 09 20               ORA #$20    |  01F7: 09 20               ORA #$20
0931: 8D 11 D0            STA $D011   |  01F9: 8D 11 D0            STA $D011   
0934: 4C 07 09            JMP j0907   |  01FC: 4C 07 01            JMP j0107
```

Only relocated, with AI-generated comments:

```
; Gruppo 1: Inizializzazione e setup dei puntatori
L0100   sei                 ; Disabilita interrupts
        lda #$34
        sta $01             ; Configura la memoria: I/O visibile, no BASIC, no KERNAL
        ldy #$00            ; Inizializza Y a 0 (usato come offset)
        lda #$f9
        sta $ae
        lda #$68
        sta $af             ; Imposta $ae-$af a $68f9 (indirizzo sorgente)
        lda #$00
        sta $ac
        sta $ad             ; Imposta $ac-$ad a $0000 (indirizzo destinazione)

; Gruppo 2: Loop principale di copia/decompressione: copia segmento di memoria $08be-$68f8 a ritroso da $FFFF in giù ,
; quindi quello che era in $08be va a finire in $9fc5 (dec40901)
L0115   lda $ac
        bne L011b
        dec $ad             ; Decrementa byte alto destinazione se necessario
L011b   dec $ac             ; Decrementa indirizzo destinazione
        lda $ae
        bne L0123
        dec $af             ; Decrementa byte alto sorgente se necessario
L0123   dec $ae             ; Decrementa indirizzo sorgente
        lda ($ae),y         ; Carica byte da sorgente
        sta ($ac),y         ; Memorizza byte a destinazione
        lda $ae
        cmp #$be
        bne L0115
        lda $af
        cmp #$08
        bne L0115           ; Continua finché sorgente = $08be
        lda #$01
        sta $ae
        lda #$08
        sta $af             ; Reimposta sorgente a $0801

; Gruppo 3: Elaborazione dati (probabile decompressione)
L013d   lda ($ac),y         ; Legge byte da destinazione
        cmp #$bf
        bne L0155           ; Se non è $bf, salta
        jsr L0178           ; Incrementa destinazione
        lda ($ac),y         ; Legge contatore
        tax                 ; Mette contatore in X
        lda #$00
L014b   sta ($ae),y         ; Scrive 0 a indirizzo sorgente
        jsr L017f           ; Incrementa indirizzo sorgente
        dex
        bne L014b           ; Ripete per il numero di volte in contatore
        beq L016b           ; Salta sempre

L0155   cmp #$cf
        bne L0166           ; Se non è $cf, salta
        jsr L0178           ; Incrementa destinazione
        lda ($ac),y         ; Legge primo byte (possibile contatore)
        tax
        jsr L0178           ; Incrementa destinazione
        lda ($ac),y         ; Legge secondo byte (possibile valore da copiare)
        bne L014b           ; Se non zero, torna a scrivere

L0166   sta ($ae),y         ; Scrive byte a indirizzo sorgente
        jsr L017f           ; Incrementa indirizzo sorgente

L016b   jsr L0178           ; Incrementa indirizzo destinazione
        bne L013d           ; Continua loop se non zero
        lda #$37
        sta $01             ; Ripristina configurazione memoria normale
        cli                 ; Riabilita interrupts
        jmp L00d5           ; Salta a $00d5 (probabile inizio del programma decompresso)

; Gruppo 4: Subroutine di incremento puntatori
L0178   inc $ac             ; Incrementa byte basso destinazione
        bne L017e
        inc $ad             ; Incrementa byte alto destinazione se necessario
L017e   rts

L017f   inc $ae             ; Incrementa byte basso sorgente
        bne L0185
        inc $af             ; Incrementa byte alto sorgente se necessario
L0185   rts

; Gruppo 5: Dati (probabile programma BASIC)
L0186   .byte $0b, $08      ; Puntatore alla prossima linea BASIC
        .byte $0a, $00      ; Numero di linea (10)
        .byte $9e           ; Token SYS
        .ascii "2061"       ; Indirizzo per SYS
        .byte $bf           ; Token PEEK
        .byte $03           ; Fine linea BASIC

; Gruppo 6: Inizializzazione hardware e configurazione sistema
L0191   lda #$00
        sta d020_vBorderCol ; Imposta colore bordo a nero
        sta d021_vBackgCol0 ; Imposta colore sfondo a nero
        jsr S0960           ; Chiama subroutine (non visibile qui)
        lda $dc0e
        and #$fe
        sta $dc0e           ; Configura CIA1 timer
        sei                 ; Disabilita interrupts
        lda $01
        and #$fc
        ora #$01
        sta $01             ; Configura bank switching
        lda #$f6
        ldy #$0a
        sta $fffe
        sty $ffff           ; Imposta vettore IRQ/BRK a $0af6
        lda #$ad
        ldy #$10
        sta $fffc
        sty $fffd           ; Imposta vettore RESET a $10ad
        sta $fffa
        sty $fffb           ; Imposta vettore NMI a $10ad
        lda #$00
        ldx #$b2
L01cb   sta $00,x           ; Azzera memoria da $00 a $b2
        dex
L01cf = * + 1       
        cpx #$02
        bne L01cb
        sta d015_vSprEnable ; Disabilita tutti gli sprite
        sta $dc02           ; Configura CIA1 porta A come input
        sta $dc03           ; Configura CIA1 porta B come input
        lda $dd02
        ora #$03
        sta $dd02           ; Configura CIA2 porta A
        lda $dd00
        and #$fc
        ora #$02
        sta $dd00           ; Seleziona VIC bank 1 ($4000-$7fff)
        lda #$78            ; #$70 OR #$08
        sta d018_vMemControl ; Configura layout memoria video
        lda $d011
        ora #$20
        sta $d011           ; Abilita schermo in modalità testo
        jmp L01cf           ; Loop infinito
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




