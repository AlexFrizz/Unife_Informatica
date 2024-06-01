| Lezione Precedente | [[14 - Strutture Dati - Heap, HeapSort e Code di priorità]] |
| ------------------ | ----------------------------------------------------------- |
| Lezione Successiva | [[16 - Strutture Dati - Insiemi Disgiunti]]                 |
_gli appunti fanno riferimento alla lezione 16 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Tabelle Hash
>Sono strutture dati astratte, che dato un *numero di oggetti relativamente piccolo*, ognuno dei quali è denotato da una chiave il cui *universo U è relativamente grande* vogliamo allocare in maniera dinamica questi oggetti
>le caratteristiche sono:
>- struttura dinamica
>- parzialmente compatte
>- non basate sull'ordinamento

![[Pasted image 20240525084020.png]]

### Tabelle hash ad accesso diretto
in questa implementazione viene utilizzato un array di puntatori T, T[key] punta all'oggetto la cui chiave è assegnata a key, questa implementazione tuttavia è poco efficiente perchè dipende dalla grandezza delle chiavi e la complessità cresce con esse.

### Tabelle hash con chaining 
questa soluzione prevede di avere un array di *m* posizioni, qualora volessimo memorizzare una chiave *k* molto grande implementiamo una **funzione di hash** h: U {1, ..., m} per poter indirizzare l'elemento k alla posizione h(k)
potremmo però trovarci in una situazione di conflitto in cui le due chiavi hashed sono uguali

per minimizzare i conflitti abbiamo questa struttura, ogni posizione dell'array punta ad una lista doppiamente collegata che inizialmente è *nil* dove gli elementi hashed vengono inseriti (sempre a partire dalla testa)

![[Pasted image 20240525085500.png]]
essendo parzialmente basata sulle liste una volta applicata la funzione di hash per trovare la posizione corretta l'inserimento viene effettuato in testa

![[Pasted image 20240525085633.png]]
allo stesso modo avviene la ricerca quindi tramite procedura per le liste

![[Pasted image 20240525085729.png]]
anche nel caso di cancellazione l'elemento viene cercato, in maniera sequenziale prima si cerca l'elemento e poi lo si elimina aggiornando i puntatori

#### Correttezza e Complessità
correttezza e terminazione non sono in discussione perchè dipendono da quelle delle liste

Complessità:
fissato *n* come numero di elementi inseriti in T e *m* come dimensione di T, se tutti gli *n* elementi finiscono nella stessa casella le operazioni di ricerca, e cancellazione sono nel **caso peggiore** e  la loro complessità è $$Θ(n)$$per vedere il **caso medio** assumiamo di avere hashing uniforme semplice, che quindi gli elementi siano inseriti in modo uniformemente sparso quindi la probabilità che un elemento venga inserito in un determinato slot è uguale per tutti quindi *n/m* (fattore di carico), detto questo la complessità sia in caso di successo che di fallimento diventa $$Θ(1+\frac{n}{m})$$
### Funzioni di Hash
esistono tre metodi principali per poter applicare una funzione di hash che sono:
- **metodo della divisione** (modulo *m*), a patto che m sia numero primo e lontano da una potenza di 2
	![[Pasted image 20240525091309.png]]
- **metodo della moltiplicazione** con m=2^p per qualche p per chiavi naturali
	![[Pasted image 20240525091336.png]]
	A è una costante compresa tra 0 e 1 reale
- **metodo dell'addizione** utilizzato per chiavi stringhe o oggetti complessi, (modulo m)
	![[Pasted image 20240525091722.png]]
	dove B è la cardinalità (*vedi approfondimenti*)


![[Pasted image 20240525091917.png]]
la procedura fa riferimento al codice del metodo dell'addizione la cui complessità dipende dalla lunghezza della parola $$Θ(d)$$i parametri sono: 
- w = a1a2...ad (parola qualsiasi)
- B (base cioè dimensione dell'alfabeto)
- m (numero primo tale che *(m + 1) * B* possa essere memorizzato in una parola di memoria)

### Tabelle hash con open hashing
l'indirizzamento aperto si basa sull'idea di provare più di una posizione sulla tabella, finchè se ne trova una libera oppure si ha la certezza che la tabella è piena

quindi, data una chiave *k* da inserire, possiamo usare un funzione di hashing per trovare una posizione ed ottenere *h(k)* se la posizione è già occupata si riprova un altra posizione (**sequenza di probing**) prima o poi tutte le posizioni della tabella devono essere provate, abbiamo quindi *h(k, i)* che dipende dalla chiave ma anche dalla posizione precedentemente tentata

![[Pasted image 20240525092944.png]]
vediamo come in entrambe le procedure vi sia un ciclo che prova tutte le posizioni per vedere nel primo caso una posizione libera, nel secondo caso se l'elemento è trovato e poi restituiscono la posizione dell'elemento

per quanto riguarda l'operazione di eliminazione, questa avviene impostando un flag alle posizioni dell'array, 1 se l'elemento è virtualmente eliminato e 0 se è pieno, perchè andare effettivamente a eliminare l'elemento potrebbe portare dei problemi nelle fasi di inserimento e ricerca

