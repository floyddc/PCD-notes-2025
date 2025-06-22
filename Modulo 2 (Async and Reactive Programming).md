# ASYNCHRONOUS PROGRAMMING
Stile di programmazione che si basa su computazioni/richieste/processi asincroni.<br>
Le architetture più utilizzate sono:
- **Task-oriented**
- **Event-driving**

## Task-oriented frameworks
Si basano sui task, ovvero un'entità:
- di _alto livello_ 
- astratta
- discreta
- indipendente
- eseguita in modo asincrono da un **thread** (entità invece di _basso livello_), quindi c'è asincronia tra esecuzione di un task e flusso di controllo che l'ha creato e avviato.
- che rappresenta una divisione logica del lavoro, invece di una concorrenza fisica.

Si identificano due figure:
- **Master**: invia i task.
- **Worker**: esegue il task

Per collezionare i risultati dei task si utlizzano:
- Bag of results
- Collector/aggregator monitor
- **Future**: meccanismo che che permette di ottenere subito un oggetto che rappresenta un risultato futuro. _E' il meccanismo chiave che spiega il perchè dell'asincronia_. Consente inoltre di controllare lo stato del task, bloccarlo, cancellarlo.

## Event-driven programming

