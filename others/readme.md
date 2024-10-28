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

[](https://github.com/jumpjack/ChoplifterReverseWithAI/blob/main/others/SIDE%20PACMAN.PRG)

- File is 24826 ($60FA) bytes long (offsets $0000-$60f9);
- Bytes at offsets $0000, $0001 are not stored but used to determine start load address ($0801);
- Remaining bytes (offsets $0002-$60f9 = 24824 bytes) get loaded from $0801 to  $68f8 (=26872 = $0801 + 24824-1), i.e. stored starting from BASIC area; indeed the initial loader is BASIC line "sys 2087";
- After loading the basic program from disk, it is automatically started and it jumps to assembly loader $0827 (sys 2087);
- Assembly loader moves block $0838-$0937 to area $0100-$01ff , beacuse programs in page zero run faster, and then it jumps to $0100 to execute the loader;
- Program starting from $0100 moves block $08be-$68f8 to area $9FC5-$ffff;
- After moving data to the end of RAM space, program reads back such data, this time applying RLE decompressing algorithm while writing from $0801 on; this results in a new BASIC program being stored from $0801 to $080c with a "SYS 2061" statement, and a new assembly program starting at $080d (decimal 2061), which is the actual game, which begins setting to black screen and border:

```
 $080d lda #$00            ; 
       sta d020            ; Black border (poke 53280,0)
       sta d021            ; Black background (poke 3281,0)
 ```

### Basic loader

`2204 SYS 2087 SHORT VERSION BY MR.Z    `

dec 2087 is $0827

### Hacking:
Right after loading, programs gets launched, but if in the emulator, before loading, you put these breakpoints, you can monitor separately the loading/decompression cycles:

- $0827 Start of the assembly programs, which moves loader to $0100 for faster execution
- $0135 End of first cycle (copy $08be-$68f8 to  $9fc5-$ffff)
- $0170 End of second cycle (decompression), main program is launched.




### Loaded PRG:

#### BASIC part
```
0801   25 08 9C 08 9E 32 30 38 37 20 53 48 4F 52 54 20 ; "SYS 2087" followed by comments
0811   56 45 52 53 49 4F 4E 20 42 59 20 4D 52 2E 5A 20
0821   20 20 20 00 00 00 
```


#### Assembly part (from $0x0827 = 2087)
Moves loader to $0100 for faster execution.
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

Loader copies block 0x0838-0x0937 (256 bytes) to 0x0100-0x01ff then jumps to 0x0100, which will contain the (relocated) code; here below is reported this code, in its original position and relocated:

```
Originale                             |   Rilocato a $0100
-------------------------------------------------------------------
0838   78                   SEI			|0100   78                   SEI
0839   A9 34                LDA #$34		|0101   A9 34                LDA #$34
083B   85 01                STA $01		|0103   85 01                STA $01
083D   A0 00                LDY #$00		|0105   A0 00                LDY #$00
083F   A9 F9                LDA #$F9		|0107   A9 F9                LDA #$F9
0841   85 AE      L0841     STA $AE		|0109   85 AE                STA $AE
0843   A9 68                LDA #$68		|010B   A9 68                LDA #$68
0845   85 AF                STA $AF		|010D   85 AF                STA $AF
0847   A9 00                LDA #$00		|010F   A9 00                LDA #$00
0849   85 AC                STA $AC		|0111   85 AC                STA $AC
084B   85 AD                STA $AD		|0113   85 AD                STA $AD
084D   A5 AC      L084D     LDA $AC		|0115   A5 AC      L0115     LDA $AC
084F   D0 02                BNE L0853		|0117   D0 02                BNE L011B
0851   C6 AD                DEC $AD		|0119   C6 AD                DEC $AD
0853   C6 AC      L0853     DEC $AC		|011B   C6 AC      L011B     DEC $AC
0855   A5 AE                LDA $AE		|011D   A5 AE                LDA $AE
0857   D0 02                BNE L085B		|011F   D0 02                BNE L0123
0859   C6 AF                DEC $AF		|0121   C6 AF                DEC $AF
085B   C6 AE      L085B     DEC $AE		|0123   C6 AE      L0123     DEC $AE
085D   B1 AE                LDA ($AE),Y		|0125   B1 AE                LDA ($AE),Y
085F   91 AC                STA ($AC),Y		|0127   91 AC                STA ($AC),Y
0861   A5 AE                LDA $AE		|0129   A5 AE                LDA $AE
0863   C9 BE                CMP #$BE		|012B   C9 BE                CMP #$BE
0865   D0 E6                BNE L084D		|012D   D0 E6                BNE L0115
0867   A5 AF                LDA $AF		|012F   A5 AF                LDA $AF
0869   C9 08                CMP #$08		|0131   C9 08                CMP #$08
086B   D0 E0                BNE L084D		|0133   D0 E0                BNE L0115
086D   A9 01                LDA #$01		|0135   A9 01                LDA #$01
086F   85 AE                STA $AE		|0137   85 AE                STA $AE
0871   A9 08                LDA #$08		|0139   A9 08                LDA #$08
0873   85 AF                STA $AF		|013B   85 AF                STA $AF
0875   B1 AC      L0875     LDA ($AC),Y		|013D   B1 AC      L013D     LDA ($AC),Y
0877   C9 BF                CMP #$BF		|013F   C9 BF                CMP #$BF
0879   D0 12                BNE L088D		|0141   D0 12                BNE L0155
087B   20 78 01             JSR $0178		|0143   20 78 01             JSR L0178
087E   B1 AC                LDA ($AC),Y		|0146   B1 AC                LDA ($AC),Y
0880   AA                   TAX			|0148   AA                   TAX
0881   A9 00                LDA #$00		|0149   A9 00                LDA #$00
0883   91 AE      L0883     STA ($AE),Y		|014B   91 AE      L014B     STA ($AE),Y
0885   20 7F 01             JSR $017F		|014D   20 7F 01             JSR L017F
0888   CA                   DEX			|0150   CA                   DEX
0889   D0 F8                BNE L0883		|0151   D0 F8                BNE L014B
088B   F0 16                BEQ L08A3		|0153   F0 16                BEQ L016B
088D   C9 CF      L088D     CMP #$CF		|0155   C9 CF      L0155     CMP #$CF
088F   D0 0D                BNE L089E		|0157   D0 0D                BNE L0166
0891   20 78 01             JSR $0178		|0159   20 78 01             JSR L0178
0894   B1 AC                LDA ($AC),Y		|015C   B1 AC                LDA ($AC),Y
0896   AA                   TAX			|015E   AA                   TAX
0897   20 78 01             JSR $0178		|015F   20 78 01             JSR L0178
089A   B1 AC                LDA ($AC),Y		|0162   B1 AC                LDA ($AC),Y
089C   D0 E5                BNE L0883		|0164   D0 E5                BNE L014B
089E   91 AE      L089E     STA ($AE),Y		|0166   91 AE      L0166     STA ($AE),Y
08A0   20 7F 01             JSR $017F		|0168   20 7F 01             JSR L017F
08A3   20 78 01   L08A3     JSR $0178		|016B   20 78 01   L016B     JSR L0178
08A6   D0 CD                BNE L0875		|016E   D0 CD                BNE L013D
08A8   A9 37                LDA #$37		|0170   A9 37                LDA #$37
08AA   85 01                STA $01		|0172   85 01                STA $01
08AC   58                   CLI			|0174   58                   CLI
08AD   4C 0D 08             JMP L080D		|0175   4C 0D 08             JMP $080D
08B0   E6 AC                INC $AC		|0178   E6 AC      L0178     INC $AC
08B2   D0 02                BNE L08B6		|017A   D0 02                BNE L017E
08B4   E6 AD                INC $AD		|017C   E6 AD                INC $AD
08B6   60         L08B6     RTS			|017E   60         L017E     RTS
08B7   E6 AE                INC $AE		|017F   E6 AE      L017F     INC $AE
08B9   D0 02                BNE L08BD		|0181   D0 02                BNE L0185
08BB   E6 AF                INC $AF		|0183   E6 AF                INC $AF
08BD   60         L08BD     RTS			|0185   60         L0185     RTS


; --------------- BASIC block ("SYS 2061", i.e. JMP 0x080D) ------------
08BE: 0B 08 0A 00 9E 32 30 36 31 BF 03  ; Note: "$BF 03" is turned into "00 00 00" by the RLE decompressor,
                                        ; which also move result to $0801 (BASIC start).
; --------------------------------------------------------------------------

Following code will start from $080d after decompression:

08C9   A9 00                LDA #$00		|0191   A9 00                LDA #$00
08CB   8D 20 D0             STA $D020		|0193   8D 20 D0             STA $D020
08CE   8D 21 D0             STA $D021		|0196   8D 21 D0             STA $D021
08D1   20 60 09             JSR L0960		|0199   20 60 09             JSR $0960
08D4   AD 0E DC             LDA $DC0E		|019C   AD 0E DC             LDA $DC0E
08D7   29 FE                AND #$FE		|019F   29 FE                AND #$FE
08D9   8D 0E DC             STA $DC0E		|01A1   8D 0E DC             STA $DC0E
08DC   78                   SEI			|01A4   78                   SEI
08DD   A5 01                LDA $01		|01A5   A5 01                LDA $01
08DF   29 FC                AND #$FC		|01A7   29 FC                AND #$FC
08E1   09 01                ORA #$01		|01A9   09 01                ORA #$01
08E3   85 01                STA $01		|01AB   85 01                STA $01
08E5   A9 F6                LDA #$F6		|01AD   A9 F6                LDA #$F6
08E7   A0 0A                LDY #$0A		|01AF   A0 0A                LDY #$0A
08E9   8D FE FF             STA $FFFE		|01B1   8D FE FF             STA $FFFE
08EC   8C FF FF             STY $FFFF		|01B4   8C FF FF             STY $FFFF
08EF   A9 AD                LDA #$AD		|01B7   A9 AD                LDA #$AD
08F1   A0 10                LDY #$10		|01B9   A0 10                LDY #$10
08F3   8D FC FF             STA $FFFC		|01BB   8D FC FF             STA $FFFC
08F6   8C FD FF             STY $FFFD		|01BE   8C FD FF             STY $FFFD
08F9   8D FA FF             STA $FFFA		|01C1   8D FA FF             STA $FFFA
08FC   8C FB FF   L08FC     STY $FFFB		|01C4   8C FB FF   L01C4     STY $FFFB
08FF   A9 00                LDA #$00		|01C7   A9 00                LDA #$00
0901   A2 B2                LDX #$B2		|01C9   A2 B2                LDX #$B2
0903   95 00      L0903     STA $00,X		|01CB   95 00      L01CB     STA $00,X
0905   CA                   DEX			|01CD   CA                   DEX
0906   E0 02                CPX #$02		|01CE   E0 02                CPX #$02
0908   D0 F9                BNE L0903		|01D0   D0 F9                BNE L01CB
090A   8D 15 D0             STA $D015		|01D2   8D 15 D0             STA $D015
090D   8D 02 DC             STA $DC02		|01D5   8D 02 DC             STA $DC02
0910   8D 03 DC             STA $DC03		|01D8   8D 03 DC             STA $DC03
0913   AD 02 DD             LDA $DD02		|01DB   AD 02 DD             LDA $DD02
0916   09 03                ORA #$03		|01DE   09 03                ORA #$03
0918   8D 02 DD             STA $DD02		|01E0   8D 02 DD             STA $DD02
091B   AD 00 DD             LDA $DD00		|01E3   AD 00 DD             LDA $DD00
091E   29 FC                AND #$FC		|01E6   29 FC                AND #$FC
0920   09 02                ORA #$02		|01E8   09 02                ORA #$02
0922   8D 00 DD             STA $DD00		|01EA   8D 00 DD             STA $DD00
0925   A9 70                LDA #$70		|01ED   A9 70                LDA #$70
0927   09 08                ORA #$08		|01EF   09 08                ORA #$08
0929   8D 18 D0             STA $D018		|01F1   8D 18 D0             STA $D018
092C   AD 11 D0             LDA $D011		|01F4   AD 11 D0             LDA $D011
092F   09 20                ORA #$20		|01F7   09 20                ORA #$20
0931   8D 11 D0             STA $D011		|01F9   8D 11 D0             STA $D011
0934   4C 07 09             JMP L0907		|01FC   4C 07 09             JMP $0907

```

### Relocated code commented (in Italian) with the help of AI

- In un primo momento il codice copia il blocco $08be-$68f8 (24634 byte) e lo incolla a partire da $9fc5 fino a $ffff, procedendo al contrario.
- in una seconda fase il loader copia da dove è arrivato il contatore ($9fc5) e incolla a partire da $0801, ma anzichè fare una copia diretta effettua una decompressione RLE:

#### Valore $BF
Ogni volta che trova un valore $BF seguito da un altro valore, nella destinazione scrive un numero di zeri pari al valore letto. Quindi la sequenza:

 08BE: 0B 08 0A 00 9E 32 30 36 31 **BF** **_03_** A9 00

verrà decompressa in:

 08BE: 0B 08 0A 00 9E 32 30 36 31 **_00 00 00_** A9 00

e poi trasferita in $0801:
```
       01 02 03 04 05 06 07 08 09 04 0b 0c 0d 0e
 0801: 0B 08 0A 00 9E 32 30 36 31 00 00 00 A9 00
```
Il "00 00 00" è il marker di fine basic, mentra "A9 00", che si traduce in "LDA #$00", si troverà in 080D, che è proprio l'indirizzo di salto del SYS, nonchè l'indirizzo di salto alla fine del loader.

#### Valore $CF
Ogni volta che invece trova un valore $CF, legge un secondo valore che sarà il numero di ripetizioni, e un terzo valore che sarà il valore da ripetere.

-----------

Codice commentato:

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

; Gruppo 2: Loop principale di copia/decompressione: copia segmento di memoria $08be-$68f8 a ritroso da $FFFF in giù fino a $9fc5. Il puntatore-sorgente è $ae,$af, la destinazione $ac,$ad.

L0115   lda $ac
        bne L011b
        dec $ad             ; Decrementa byte alto destinazione.
L011b   dec $ac             ; Decrementa byte basso destinazione.
                            ; Alla prima esecuzione quindi la destinazione parte da $FFFF.    
        lda $ae
        bne L0123
        dec $af             ; Decrementa byte alto sorgente.
L0123   dec $ae             ; Decrementa indirizzo sorgente.
                            ; Alla prima esecuzione quindi parte da $68f9-$0001, quindi $68f8.   
        lda ($ae),y         ; Carica byte da sorgente.
        sta ($ac),y         ; Memorizza byte in destinazione.
        lda $ae
        cmp #$be            ; Continua finché indirizzo sorgente = $08be; contatore Y inutilizzato.
        bne L0115
        lda $af
        cmp #$08
        bne L0115           


; -------------
; Gruppo 3: Seconda copia (e altro?): scrive in area BASIC programma presente inizialmente
; da $08be (ora in $9fc5)
; -------------
        lda #$01            ;  $ae,$af diventa la destinazione e viene impostato a $0801.
        sta $ae
        lda #$08
        sta $af             
L013d   lda ($ac),y         ; Legge byte da $ac,$ad, dove è arrivata la routine di prima ($9fc5) finchè
                            ; non trova il marker $bf.
        cmp #$bf
        bne L0155           ; Se non è $BF, controlla se è $CF: se non lo è, scrive in
                            ; destinazione ($ae,$af) e la incrementa

                            ; Se è $BF:
; ---- ciclo per $BF: scrive un certo numero di zeri ----
        jsr L0178           ; Incrementa $ac,$ad (sorgente)
        lda ($ac),y         ; Legge da $ac,$ad + Y
        tax                 ; Mette in X il valore appena letto, per usarlo come contatore

        lda #$00            ; Scrive un numero di zeri pari al valore di X, dopodichè tramite il
                            ; beq L016b esce da questo loop e torna a quello principale di controllo/copiatura L103d.

; ---- Ciclo per $CF: scrive un valore un certo numero di volte ---
L014b   sta ($ae),y         ; Scrive in $ae,$af + Y: scrive uno zero se il flusso arriva qui dal BNE qui sotto,
                            ; ma scrive il valore letto dalla memoria se invece torna qui dalla subroutine L0155.
        jsr L017f           ; Incrementa $ae,$af (destinazione)
        dex                 ; X--  ;  decrementa  contatore
        bne L014b           ; Ripete per il numero di volte in contatore

        beq L016b           ; Quando il contatore arriva a zero, prosegue saltando la subroutine L0155 qui sotto
                            ; e andando direttamente a $016b che incrementa il punatore-sorgente


; ---- subroutines ---
L0155   cmp #$cf
        bne L0166           ; Se non è $cf, salta

                            ; Se è CF:
        jsr L0178           ; Incrementa destinazione
        lda ($ac),y         ; Legge primo byte e
        tax                 ; lo memorizza come contatore.
        jsr L0178           ; Incrementa destinazione
        lda ($ac),y         ; Legge secondo byte (valore da copiare)
        bne L014b           ; Se non zero, torna a scrivere

L0166   sta ($ae),y         ; Scrive in $ae,$af
        jsr L017f           ; Incrementa $ae,$af 

; ------ fine subroutines ---------


L016b   jsr L0178           ; Incrementa $ac,$ad
        bne L013d           ; Continua loop se non zero. 

; ------ fine loop ---------



        lda #$37            ; *** Si può mettere un BRK qui ($0170) per esaminare 
                            ; il risultato del secondo ciclo di copia. *** 
        sta $01             ; Ripristina configurazione memoria normale
        cli                 ; Riabilita interrupts
        jmp L080D           ; Salta a L080D  (2061, la stessa posizione del SYS del nuovo programma
                            ; BASIC, che inizialmente partiva da $08BE, ma chè stato rilocato e
                            ; decompresso in $0801; dall'originale:

      BE BF C0 C1 C2 C3 C4 C5 C6 C7 C8 C9 CA
 8BE: 0B 08 0A 00 9E 32 30 36 31 BF 03 A9 00

                            ; diventerebbe:

      01 02 03 04 05 06 07 08 09 04 0b 0c 0d 0e
 0801: 0B 08 0A 00 9E 32 30 36 31 BF 03 A9 00


                            ; Ma in realtà la decompressione trasforma $BF in "00 00 00", quindi:

 
      01 02 03 04 05 06 07 08 09 04 0b 0c 0d 0e
 0801: 0B 08 0A 00 9E 32 30 36 31 00 00 00 A9 00


 Il "00 00 00" è il marker di fine basic, mentra "A9 00", che si traduce in "LDA #$00", si troverà in 080D, che è proprio 
 l'indirizzo di salto del SYS, nonchè l'indirizzo di salto alla fine del loader.


; ---------- Gruppo 4: Subroutine di incremento puntatori ---------
L0178   inc $ac             ; Incrementa byte basso 
        bne L017e
        inc $ad             ; Incrementa byte alto  se necessario
L017e   rts

L017f   inc $ae             ; Incrementa byte basso 
        bne L0185
        inc $af             ; Incrementa byte alto se necessario
L0185   rts

; Gruppo 5: Dati (programma BASIC (era in $08be al caricamento del .prg, alla fine si trova in $0801))
L0186   .byte $0b, $08      ; Puntatore alla prossima linea BASIC
        .byte $0a, $00      ; Numero di linea (10)
        .byte $9e           ; Token SYS
        .ascii "2061"       ; Indirizzo per SYS
        .byte $bf           ; marker per decompressione RLE: decodifica in un numero di zeri pari al prossimo numero (3)
        .byte $03           ; "00 00 00": fine programma BASIC.


; Gruppo 6: Inizializzazione hardware e configurazione sistema
L0191   lda #$00            ; (era in $08c9, alla fine si trova in $080d, dec2061)
        sta d020_vBorderCol ; Imposta colore bordo a nero
        sta d021_vBackgCol0 ; Imposta colore sfondo a nero
        jsr e8DD0           ; Chiama subroutine (non visibile qui)
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




