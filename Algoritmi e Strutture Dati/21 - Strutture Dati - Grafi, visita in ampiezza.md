| Lezione Precedente | [[20 - Strutture Dati - BT]]                          |
| ------------------ | ----------------------------------------------------- |
| Lezione Successiva | [[22 - Strutture Dati - Grafi, visita in profondità]] |
_gli appunti fanno riferimento alla lezione 25 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Grafi: Visita in Ampiezza e problemi collegati
>Un Grafo è una coppia *G = (V, E)* composta da un insieme V di vertici e un insieme E di archi
>hanno le seguenti caratteristiche:
>- la struttura dati è *statica*
>- *non basata sull'ordinamento*
>- rappresentata in maniera *sparsa o parzialmente compatta* a seconda del caso

i grafi possono essere:
- **indiretti:** se sono rappresentati con archi senza direzione
- **diretti:** se gli archi hanno una direzione

![[Pasted image 20240526114323.png]]

- **grado entrante:** di un nodo di un grafo diretto è il numero di archi che lo raggiungono
- **grado uscente:** di un nodo di un grafo diretto è il numero di archi che lo lasciano

un grafo (diretto o indiretto) può essere **pesato** dove per ogni arco è assegnato un valore numerico definito come peso
![[Pasted image 20240526114602.png]]
nel caso di grafi pesati la coppia V, E diventa *G = (V, E, w)* dove *w* rappresenta il valore dei pesi

possiamo anche pensare che tutti i grafi siano pesati, solo assegnando peso 1 agli archi dove in realtà il grafo è non pesato

un grafo si distingue anche tra:
- **sparso:** qualora il numero di archi è molto minore del numero di archi possibili |E|<<|V|^2 (il massimo numero di archi è dato dal numero di vertici al quadrato)
- **denso:** quando il numero di archi è simile al numero di archi possibili 

- un grafo sparso è meglio rappresentato da una **lista di adiacenza**
- un grafo sparso è meglio rappresentato da una **matrice di adiacenza**

![[Pasted image 20240526115719.png]]
in questo esempio il grafo non è pesato, ma la rappresentazione sarebbe analoga, con le seguenti differenza:
- *liste di adiacenza*: aggiungiamo alle celle della lista anche il valore del peso su quel ramo
- *matrice di adiacenza*: invece di utilizzare lo zero utilizziamo un valore null e invece del valore 1 utilizziamo il peso di quel ramo

Aggiungiamo inoltre che ogni vertice avrà i seguenti attributi:
- **colore:** che può assumere i valori Bianco (non ancora scoperto), Grigio (scoperto parzialmente) o Nero (scoperti tutti i suoi rami)
- **predecessore:** puntatore al vertice precedente
- **distanza:** valore di distanza dalla sorgente (nodo dal quale comincia la visita)

### Visita in Ampiezza
per via della natura statica dei grafi non parleremo di inserimento e cancellazione ma vedremo solo le operazioni di visita

*vogliamo sapere quanti archi sono necessari a raggiungere qualunque altro vertice raggiungibile da s (sorgente)*

ci aiutiamo con la struttura dati della coda nella quale inseriamo ed estraiamo, ovviamente con poitica FIFO, i vertici una volta che vengono scoperti e analizzati

![[Pasted image 20240526141550.png]]
analizzando la procedura di visita notiamo subito che 
- nella prima parte vi è un ciclo for che imposta tutti i vertici (a partire dalla sorgente) di colore bianco, con distanza infinito e senza predecessore
- poi vengono aggiunte le informazioni relative al nodo sorgente e viene inserito nella coda
- fintanto che la coda non è vuota, il vertice viene estratto dalla coda (inizialmente la sorgente)
- vengono analizzati tutti i nodi adiacenti (con una politica a scelta per determinarne in quale ordine)
	- se il nodo è bianco allora vengono aggiornate le sue informazioni 
	- se è non lo è allora diventa nero

![[Pasted image 20240526142351.png]]
*l'immagine mostra la procedura di visita in ampiezza per il grafico descritto*

#### Correttezza e Complessità:
Terminazione:
si può facilmente dimostrare analizzando la procedura 

Correttezza:
l'invariante che usiamo in questo caso è il seguente: *appena un vertice v entra in Q, si ha che v.d = δ(s, v); (distanza minima tra sorgente e vertice)*

Complessità:
come possiamo intuire la complessità dei grafi dipende dalla loro struttura e dai cicli della procedura quindi:
- il primo for scorre tutti i vertici V quindi $$Θ(|V|)$$il secondo ciclo più esterno invece, il while, scorre parte o tutti gli archi E quindi $$Θ(|E|)$$e quindi la complessità totale nel **caso medio** diventa $$Θ(|V| + |E|)$$mentre nel **caso peggiore** in qui il grafo è connesso e denso (quindi i rami sono pari al numero di vertici al quadrato) abbiamo che $$Θ(|V^2|)$$