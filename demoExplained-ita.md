# Spiegazione della modalità demo di Choplifter

Questo codice implementa la modalità demo del gioco Choplifter. La modalità demo è una versione semplificata del ciclo di gioco principale, progettata per mostrare il gameplay senza l'input del giocatore.

## Struttura principale

Il ciclo principale della demo è implementato nella routine `mainLoopDemo`. Ecco come funziona:

1. Incrementa il contatore dei frame (`ZP_FRAME_COUNT`).
2. Controlla se la demo ha raggiunto i 224 frame (0xE0 in esadecimale).
3. Se non ha raggiunto i 224 frame, passa al controllo della morte (`mainLoopDemoDeathCheck`).
4. Se ha raggiunto i 224 frame, controlla lo stato dell'azione demo (`mainLoopDemoAction`).
5. Se l'azione demo è positiva (andata), passa a `mainLoopDemoHeadHome`.
6. Se l'azione demo è negativa (ritorno), salta alla sequenza del titolo.

## Azioni della demo

La demo è divisa in due fasi:
1. Andata (224 frame): l'elicottero vola verso i nemici/ostaggi.
2. Ritorno (224 frame): l'elicottero torna alla base.

Queste fasi sono controllate dalla variabile `mainLoopDemoAction`:
- 0x00: Andata
- 0xFF: Ritorno

## Tabella di controllo del volo

La demo utilizza una tabella predefinita (`mainLoopDemoTable`) per controllare il movimento dell'elicottero. Ogni entry nella tabella rappresenta un frame e contiene 4 byte:

1. Velocità Y
2. Accelerazione X
3. Direzione di fronte
4. Azione di sparo

La stessa tabella viene utilizzata sia per l'andata che per il ritorno, ma i valori vengono interpretati in modo diverso basandosi sul valore di `mainLoopDemoAction`.

## Gestione del ritorno

Quando la demo raggiunge i 224 frame, entra nella fase di ritorno:

1. Imposta `mainLoopDemoAction` a 0xFF (ritorno).
2. Azzera la velocità X e Y.
3. Imposta la velocità Y a -2 (0xFE) per iniziare il movimento verso l'alto.

## Gestione della morte

Anche se non dovrebbe accadere nella demo, c'è una gestione per la morte dell'elicottero:

1. Se `ZP_DYING` è impostato, tutti gli input di controllo vengono azzerati.
2. L'accelerazione Y viene impostata a 2, probabilmente per far cadere l'elicottero.

## Osservazioni finali

1. La demo utilizza lo stesso codice del gioco principale, ma con controlli predefiniti invece dell'input del giocatore.
2. La durata totale della demo è di 448 frame (224 * 2), utilizzando la stessa tabella di controllo in entrambe le direzioni.
3. Il codice include una gestione della morte, anche se non dovrebbe verificarsi durante la demo, forse come precauzione contro comportamenti imprevisti.

Questo approccio alla modalità demo è ingegnoso perché riutilizza gran parte del codice del gioco principale e utilizza una singola tabella di controllo per entrambe le fasi della demo, risparmiando spazio prezioso nella memoria limitata dei computer dell'epoca.
