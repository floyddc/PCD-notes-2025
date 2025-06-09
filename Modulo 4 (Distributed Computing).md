# DISTRIBUTED SYSTEMS
Insieme di processori connessi in rete.

## Vantaggi
- Scalabilità
- Modularità ed eterogeneità
- Condivisione dati e risorse
- Struttura geografica
- Basso costo

## Caratteristiche
- Assenza di clock condiviso
- Assenza di memoria condivisa
- Assenza di rilevatore di errori 

## Teorema CAP
Qualsiasi sistema connesso in rete con dati condivisi deve avere almeno 2 di queste 3 proprietà:
- (`C`) **Consistency**: tutti i dati nei vari nodi del sistema non presentano incongruenze. Se un dato è stato aggiornato in un nodo, tutti gli altri dovranno essere aggiornati immediatamente. 
- (`A`) **Availability**: a qualunque richiesta deve corrispondere una risposta (costante disponibilità di dati), anche se parte del sistema non è funzionante.
- (`P`) **Partition tolerance**: il sistema deve continuare a funzionare anche se parte di esso non è funzionante.

## Latenza
Ritardo nella comunicazione. Nel teorema CAP viene apparentemente ignorata, ma influenza molto la **partition tolerance**. 

## Problemi e limiti della computazione distribuita
- Programmazione OOP
- Programmazione procedurale

## Modello di computazione distribuita
Consiste in un **sistema asincrono** composto da un insieme di processi, i quali:
- Sono composti a loro volta da stati ed eventi che alterano gli stati stessi.
- Inviano messaggi attraverso canali unidirezionali, con buffer infiniti e privi di errori.
- Ritardi arbitrari (ma finiti)
- Nessuna assunzione sull'ordinamento dei messaggi. 

## Modelli utilizzati - Interleaving
Si assume ordinamento totale tra gli eventi (tempo fisico).

## Modelli utilizzati - Happened before
Si assume ordinamento parziale tra gli eventi (tempo logico). 
  - **Causalità diretta**: se (_e_ ⋞ _f_) oppure (_e_ ↝ _f_), allora _e_ → _f_:
    - (_e_ ⋞ _f_) vuol dire che _e_ avviene prima di _f_, nello stesso processo.
      - **Esempio**: 
    - (_e_ ↝ _f_) vuol dire che _e_ avviene prima di _f_, in processi diversi, e comunicano tramite messaggi.
      - **Esempio**: _e_ è l'invio di un messaggio nel processo P1, _f_ è la ricezione del messaggio nel processo P2.
  - **Transitività**: se esiste _g_ tale che ( _e_ → _g_) e ( _g_ → _f_), allora  _e_ → _f_.

## Modelli utilizzati - Happened before - Meccanismi implementativi
- Logical clocks: una funzione _C_ di un processo _Pi_ assegna un numero _Ci(a)_ ad ogni evento _a_, secondo la **clock condition**:
  - Se (_a_ → _b_) allora (_Ci(a)_ → _Cj(b)_), con _a_ evento del processo _Pi_ e _b_ evento del processo _Pj_.
  E' inoltre sempre vero che se (_a_ → _b_) allora (_Ci(a)_ < _Cj(b)_), quindi se un evento è happened before un altro, allora il valore del clock logico del primo evento è minore del secondo. Ma **non è vero il contrario**
- Vector clock: risolvono la limitazione dell'affermazione precedente. Un vettore _VC_, di dimensione uguale al numero di processi considerati, viene assegnato ad ogni evento.
  - (_a_ → _b_) ⟺ (_VC(a)_ < _VC(b)_)

## Modelli utilizzati - Potential causality
L'idea di base è che non tutti gli eventi di un sistema concorrente siano direttamente correlati tra loro. Alcuni di essi possono essere ad esempio causalmente in relazione oppure indipendenti/concorrenti.
  - Concorrenza tra eventi: (_e_ || _f_) = not (_e_ → _f_) and not (_f_ → _e_).

## Problemi dei sistemi distribuiti
L'assenza di un clock condiviso, di risorse condivise e il dover gestire i fallimenti, comportano il dover affrontare i seguenti problemi:
- **Mutua esclusione** tra processi distruibuiti, ma bisogna comunque rispettare i principi di safety, liveness e fairness.
  - **Soluzione con algoritmo centralizzato**: si utilizza un coordinatore, il quale gestisce le richieste secondo la relazione happened before e impiega i vector clock. Il coordinatore ritarda ogni richiesta finchè le precedenti non vengono ricevute.
    - **Elezione del leader/coordinatore (algoritmo di Chang-Roberts)**: viene imposta una topologia logica ad anello. Ogni processo ha un PID univoco. Alla fine il leader sarà quello col PID più alto.<br>
    L'elezione parte da un processo, il quale (se non ha ancora ricevuto un PID più alto del proprio) invia il proprio PID ad uno dei processi vicini. Se riceve un PID più alto del proprio, lo inoltra. Se riceve il suo stesso PID, allora si dichiara leader, informando tutti gli altri tramite un messaggio.
  - **Soluzione con algoritmo decentralizzato (Agrawala)**: per accedere ad una risorsa, un processo invia un messaggio con timestamp a tutti gli altri processi.
    - Se un processo _P1_ **non** è interessato a quella risorsa, invia un OK come risposta.
    - Se un processo _P1_ è interessato ma ha un timestamp **superiore** a quello ricevuto da _P2_, invia un OK come risposta e si mette in coda per accedere successivamente alla risorsa.
    - Se un processo _P1_ è interessato ma ha un timestamp **inferiore** a quello ricevuto da _P2_, allora ha la precedenza su quest'ultimo, il quale viene aggiunto alla coda dei processi in attesa.
      - _P2_ accederà alla risorsa solo quando riceverà un OK da tutti gli altri che hanno la precedenza.
- **Ordinamento dei messaggi**: essendoci massimo non-determinismo nell'ordine dei messaggi inviati, l'unica soluzione è effettuare un ordinamento causale. I vector clock vengono estesi ad una matrice _m_ di interi, la quale permette ad un processo _Pi_ di tenere del numero di messaggi inviati da un processo _Pj_ ad un altro _Pk_ (_m[j,k]_).
  - **Algoritmo**
    - Quando _Pi_ invia un messaggio a _Pj_, incrementa _m[i,j]_ e invia la matrice insieme al messaggio.
    - Un messaggio può essere ricevuto (è **eligible**) da un processo _Pj_ quando il numero di messaggi inviati da _Pi_ a _Pj_ è minore o uguale al valore presente nella matrice di _Pi_ (logicamente, i messaggi ricevuti da _Pj_ non devono essere più dei messaggi inviati da _Pi_ a _Pj_)
    - Se un messaggio non può essere ricevuto, viene aggiunto ad un buffer finchè non diventerà finalmente eligible.