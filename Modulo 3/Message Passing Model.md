# MESSAGE PASSING MODELS
## Idea di base
- Modello sempre più utilizzato **in ambito di programmazione distribuita ma anche concorrente**, perchè evita problemi come la gestione della mutua esclusione<br>
- Scambio di messaggi tra processi **tramite funzioni primitive**, **senza lock, monitor e semafori**.<br>

## Primitive
  - `chan ch (type id1, type id2, ...)` canale avente tipi e identificatori dei vari campi trasmessi (accesso al canale è operazione atomica).<br>Schemi/modelli di comunicazione:
      - **one-to-one**: canale utilizzabile solo da coppia di processi.
      - **many-to-many**: canale utilizzabile da tot mittenti e tot destinatari.
        - competizione in ricezione
        - non determinismo
      - **many-to-one**: vengono impiegate delle porte, con tot mittenti e singolo destinatario.
  - `send ch (expr1, expr2, ...)` invia messaggio composto da espressioni sul canale (tipi delle espressioni devono essere uguali ai tipi dei campi).
  - `receive ch (var1, var2, ...)` riceve messaggio sul canale (tipi delle variabili devono essere uguali ai tipi dei campi).

## Tipo di comunicazione utilizzabile
- **Sincrona**
  - Invio di messaggio => bloccante, finchè messaggio non viene ricevuto.
  - Ricezione di messaggio => bloccante, finchè non presente nuovo messaggio sul canale.
  - Go Lang utilizza questo tipo di comunicazione.
  - **Rendez-vous**: estensione della sincronizzazione, mittente aspetta sia che il destinatario riceva il messaggio, sia che risponda.
- **Asincrona**
  - Code FIFO utilizzate per i canali.
    - Invio ha successo <=> messaggio aggiunto alla coda
    - Ricezione bloccata finchè non presente nuovo messaggio sul canale.

## Produttori-Consumatori con asincronia
<img src="imgs/prodcons.png" width="60%">

## Guards 
Utilizzate nella comunicazione sincrona o asincrona per risolvere il problema di ricezione messaggi da più canali contemporaneamente. Si basa sulla **ricezione selettiva**.<br>
`B; C → S`<br>
ossia<br>
`(Boolean); (Communication statement) → (Statement block)`

## Guards con if
```
if B1; C1 → S1;
[] B2; C2 → S2;
[] B3; C3 → S3;
fi
```

Ad esempio:<br>
```
if nReqs < max; receive computeSum(a,b,i) → nReqs+=1; send result[i](a+b)
[] nReqs < max; receive computeMul(a,b,i) → nReqs+=1; send result[i](a*b)
fi
```

## Guards con cicli do
```
do B1; C1 → S1;
[] B2; C2 → S2;
[] B3; C3 → S3;
od
```

Il ciclo viene ripetuto finchè non guardia non cede. Ad esempio:<br>
```
char buffer[10];
int front = 0, rear = 0, count = 0;

do count < 10; receive in(buffer[rear]) → count++; rear = (rear+1)%10;
[] count > 0; send out(buffer[front]) → count--; front = (front+1)%10;
od
```

## Interazione 
L'interazione che avviene per lo scambio di messaggi è **peer-to-peer**. Solitamente le informazioni e le resposabilità vengono **decentralizzate** tra i vari partecipanti, i quali eseguono tutti lo stesso codice ed alcuni di loro assumono il **ruolo di coordinatore**.

## Modelli di interazione
- **Centralizzato**: unico coordinatore che gestisce informazioni provenienti da diversi partecipanti.
  - _PRO_: numero di messaggi contenuto.
  - _CONTRO_: collo di bottiglia, ricezione del coordinatore ritardata.
- **Simmetrico**: ogni partecipante esegue le stesse funzioni inviando inizialmente i propri dati a tutti gli altri partecipanti
  - _PRO_: alta concorrenza.
  - _CONTRO_: gran numero di messaggi.
- **Ad anello**: ogni partecipante comunica solo col predecessore e col successore, creando appunto un anello.
  - _PRO_: numero di messaggi contenuto.
  - _CONTRO_: concorrenza limitata.




