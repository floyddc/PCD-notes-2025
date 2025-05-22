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