# ACTORS
- Entità reattive che comunicano tra loro tramite scambio di **mesaggi asincroni**.
- Portano la concorrenza nel paradigma OOP.
- Su un hardware fisico si può avere un numero arbitrario di attori, indipendentemente dal numero di threads fisici supportati.
- Sono caratterizzati da:
  - **stato** (_incapsulato_, un attore non può accedere allo stato degli altri)
  - **comportamento reattivo** (esegue azioni solo quando riceve messaggi) (azioni rappresentate da `handler`)
  - **flusso di controllo**
  - **coda di messaggi in arrivo**
- Possono comunicare <=> conoscono i rispettivi identificatori.

## Primitive supportate da linguaggi/framework ad attori
- `send` invia un messaggio in modo asincrono. Ha successo quando il messaggio viene inviato. Vengono fornite garanzie sull'effettiva consegna, ma non sulle tempistiche.
- `create` crea un attore con un certo comportamento.
- `become` cambia il comportamente di un attore, in base alla propria storia.
- **NON esiste** una primitiva `receive`

## Altre features
- Garantiscono **fairness** nell'invio e gestione dei messaggi.
- Garantiscono **trasparenza** sulla _posizione_ degli attori (è importante conoscere gli identificatori, non le posizioni)

<img src="imgs/actors.png" width="80%">

## Handler e Event loop 
<img src="imgs/eventLoop.png" width="60%">

- Ogni `handler` è eseguito completamente prima di ricevere un nuovo messaggio.
- Ogni `handler` dovrebbe avere un comportamento non-bloccante. Il "punto di blocco" è gestito dall'**event loop**.
- Vengono **evitate le corse critiche**.
- Complicato programmare in questo modo.