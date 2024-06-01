| Lezione Precedente | [[13 - Strutture Dati - Pile e Code]]  |
| ------------------ | -------------------------------------- |
| Lezione Successiva | [[15 - Strutture Dati - Tabelle Hash]] |
_gli appunti fanno riferimento alla lezione 15 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Min e Max Heap

> Una heap è una struttura dati astratta basata su array con le seguenti caratteristiche
> - parzialmente basata sull'ordinamento
> - necessariamente compatta 
> 
> la caratteristica principale delle heap è quella di mantenere le chiavi semi ordinate 

una min/max heap è un array H che può essere visto come un *albero binario quasi completo*, l'elemento H[1] è la radice dell'albero e distinguiamo
- *H.length:* lunghezza dell'array che contiene una heap
- *H.heapsize:* numero di elementi della heap contenuta in H

tipicamente si ha che 0 <= H.heapsize <= H.lentgh 

![[Pasted image 20240524231245.png]]
nella corretta implementazione si ha che i figli di un nodo nella posizione *i* siano precisamente gli elementi nelle posizioni 2 * i (sinistro) e così via come visto nell'immagine sopra, questo per via del parziale ordinamento nella natura delle heap

distinguiamo ora tra min heap e max heapimmaginando la struttura ad albero:
**max heap**: è tale che per ogni *i* H[Parent(i)] >= H[i] quindi ogni elemento padre ha un valore maggiore dei figli
**min heap**: è tale che per ogni *i* H[Parent(i)] <= H[i] quindi ogni elemento padre ha un valore maggiore dei figli

l'**altezza** di una heap è la lunghezza del massimo cammino (numero di archi) dalla radice ad una foglia

![[Pasted image 20240524231912.png]]

esiste una relazione tra altezza *h* e massimo numero di elementi che può contenere (si contano tutti i nodi, anche quelli precedenti), per esempio

| h   | min | max |
| --- | --- | --- |
| 0   | 1   | 1   |
| 1   | 2   | 3   |
| 2   | 4   | 7   |
la relazione in questo caso è *2^h <= n <= 2^(h+1) -1* da questa relazione possiamo analizzarne la complessità e pertanto $$h=Θ(log(n))$$

### Min/Max Heapify
come facciamo a trasformare una non heap in una heap?
![[Pasted image 20240524232646.png]]
la procedura comincia confrontando i figli sinistro e destro di un nodo *i* ne trova il più piccolo e li scambia (padre e figlio) in modo da rispettare la proprietà delle min heap e poi si richiama ricorsivamente sul sottoalbero inferiore

#### Correttezza e Complessità
Correttezza: 
essendo costruita in maniera ricorsiva (se pur tail-ricorsiva) dobbiamo trovare un invariante 
- dopo ogni chiamata a MinHeapify su un nodo di altezza *h* tale che entrambi i figli sono radici di min-heap prima della chiamata, quel nodo è la radice di una min-heap

Terminazione:
basti vedere i controlli presenti nel codice per accorgersi che la proprietà è rispettata

Complessità:
la complessità della procedura dipende dalla sua altezza essendo un albero quasi completo possiamo intuire un pattern logaritmico, comunque risolvendo la ricorrenza (vedi le slide) otteniamo che $$h=Θ(log(n))$$

### BuildMinHeap
![[Pasted image 20240524234129.png]]
dato un array di interi vogliamo convertirlo in una min-heap, impostiamo innanzitutto dimensione e numero di elementi in modo uguale e poi partendo dal basso chiamiamo ricorsivamente MinHeapify per costruire la nostra min-heap

#### Correttezza e Complessità
Correttezza: 
ad ogni iterazione del ciclo for ogni elemento preso è la radice di una min-heap

Terminazione:
la terminazione è ovvia per via della condizione nel ciclo

Complessità:
la complessità dipende in parte dalla complessità di MinHeapify che ha complessità logaritmica e per via di BuildMinHeap si esegue *n* volte, quindi potremmo dire che ha complessità $$O(n * log(n))$$ tuttavia volendo essere più precisi ed analizzando la ricorrenza otterremo che la complessità è $$Θ(n)$$

## HeapSort
data la rilevanza dell'ordinamento nella struttura heap potremmo pensare di utilizzarle in un algoritmo di ordinamento
![[Pasted image 20240524235152.png]]
sappiamo per certo che una volta eseguito BuildMaxHeap e di conseguenza MaxHeapify l'ultimo elemento sarà il più piccolo, ci basta posizionarlo all'inizio ed escluderlo dalla successiva chiamata ricorsiva per avere l'array ordinato

#### Correttezza e Complessità
Correttezza:
è immediata perchè dipende dalle procedure sulle quali è basato

Terminazione:
anche in questo caso è semplice verificare i controlli applicati al codice

Complessità:
nel caso peggiore l'array di partenza è ordinato nel verso opposto e quindi sono richieste *n* chiamate a BuildMaxHeap più la complessità logaritmica di MaxHeapify otteniamo quindi $$Θ(n * log(n))$$

## Code di Priorità
>è una struttura dati astratta, con una struttura simile alla cosa classica con politica FIFO ma ai quali elementi è applicata una priorità, ha le seguenti caratteristiche:
>- basata sull'ordinamento
>- necessariamente compatta

le operazioni possibili sono, inserimento, cambio di priorità (che può solo aumentare), trova il minimo (cioè l'elemento con la priorità maggiore)

![[Pasted image 20240525000044.png]]
l'operazione di inserimento aggiunge un nuovo elemento alla coda, qualora ci sia spazio, e poi ne aggiorna la priorità

![[Pasted image 20240525000155.png]]
questa operazione aumenta il valore di priorità di un elemento effettuando gli spostamenti necessari per correggerne la posizione

![[Pasted image 20240525000258.png]]
l'operazione trova il valore minimo quindi l'elemento con la priorità maggiore e lo elimina dalla heapsize

### Correttezza e Complessità
Correttezza: 
correttezza e terminazione sono immediate da trovare vista la semplicità delle procedure

Complessità:
nel caso pessimo di tutte le operazioni dipendendo dalla dimensione sono uguali a $$Θ(log(n))$$


*vedi code di priorità basate su array nelle slide*