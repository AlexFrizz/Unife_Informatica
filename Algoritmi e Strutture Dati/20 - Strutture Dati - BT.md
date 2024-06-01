| Lezione Precedente | [[19 - Strutture Dati - RBT]]                       |
| ------------------ | --------------------------------------------------- |
| Lezione Successiva | [[21 - Strutture Dati - Grafi, visita in ampiezza]] |
_gli appunti fanno riferimento alla lezione 22 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Alberi B
>sono strutture dati ottimizzata per minimizzare gli accessi a disco, utilizzando la memoria secondaria hanno le seguenti caratteristiche:
>- struttura *dinamica*
>- basata sull'*ordinamento*
>- memorizzata in maniera *sparsa*

un albero B (balanced) si caratterizza pe: 
- possedere una arietà (branching factor) superiore a 2 spesso dell'ordine delle migliaia
- *un'altezza proporzionale a un logaritmo* a base molto alta di n, dove n è il numero di chiavi
- per avere nodi che contengono *molte chiavi, tra loro ordinate*,
- e per crescere verso l'alto,

la complessità delle operazioni anche in questo caso è proporzionale all'altezza

In un nodo *x* il numero delle chiavi è vincolato da un parametro che si chiama *grado minimo*, si denota con *t* ed è sempre maggiore o uguale a 2 e ha le seguenti proprietà:
- ogni nodo tranne la radice ha almeno *t-1* chiavi
- ogni nodo può contenere al massimo *2t-1* chiavi
- Per ogni nodo x, x.key1 ≤ x.key2 ≤ . . . ≤ x.keyx.n;
- Per ogni nodo x, se un nodo y è contenuto nel sotto-albero radicato in *x.ci*, allora tutte le sue chiavi sono minori o uguali a *x.key_i* (se c'è)
- Per ogni nodo x, se un nodo y è contenuto nel sotto-albero radicato in *x.ci*, allora tutte le sue chiavi sono maggiori di *x.key_i−1* (se c'è)

![[Pasted image 20240525183423.png]]
*esempio con t=2* 

altre caratteristiche che ne derivano sono:
- Le prime due regole implicano che ogni nodo interno, tranne la radice, deve avere almeno *t* figli
- Se l'albero non è vuoto allora la radice ha almeno una chiave
- un nodo interno può avere fino a *2t* figli, in questo caso lo chiamiamo *nodo pieno*

Come conseguenza delle regole l'altezza di un BT con *n* chiavi e grado minimo *t ≥ 2* è:
![[Pasted image 20240525184921.png]]

### Operazioni:
nell'analizzare le operazioni e soprattutto la complessità distinguiamo tra:
- uso della CPU (come al solito)
- ed accessi al disco

#### Ricerca
![[Pasted image 20240525185110.png]]
la ricerca (che parte dal nodo radice) prende in input il puntatore del nodo di partenza e la chiave da cercare, scorriamo tutte le chiavi dei nodi sullo stesso livello fino a quando non troviamo un elemento maggiore della chiave allora scendiamo di livello e si chiama ricorsivamente
- se il nodo viene trovato si restituisce il puntatore
- se il nodo non viene trovato e siamo in una foglia allora non esiste

ad ogni chiamata si legge sul disco il livello successivo

Complessità:
come detto distinguiamo tra CPU ed accessi al disco 
- **Acessi al Disco:** avviene una DiskRead ogni volta che scendiamo di livello quindi è strettamente legata all'altezza e quindi $$O(h)=O(log_t(n))$$
- **CPU:** in maniera simile anche qui dipendiamo dall'altezza ma anche dal grado minimo *t* quindi $$Θ(t*h)=Θ(t*log_t(n))$$
#### Inserimento
![[Pasted image 20240525190057.png]]
prima di fare qualsiasi inserimento dobbiamo assicurarci che l'albero esista (vuoto), a questo scopo chiamiamo la procedura Allocate() per creare spazio sufficiente nella memoria secondaria
assumiamo che tutta questa operazione richieda tempo costante

![[Pasted image 20240525190521.png]]
la procedura prende in input il nodo *x* e il punto di split *i* alloca un nuovo nodo *z* al qual assegna la stessa caratteristica (leaf o notleaf) del nodo *y* poi avvengono le seguenti operazioni:
- vengono spostati *t-1* chiavi nel nuovo nodo *z*
- se il nodo non è foglia allora gli vengono assegnati i puntatori corretti
- *z* diventa un nuovo figlio di *x* assumendo che *x* non sia pieno 
- vengono portati al nodo *x* le chiavi necessarie di *y*
- vengono poi aggiornati i puntatori con le operazioni di accesso al disco

![[Pasted image 20240525191624.png]]

![[Pasted image 20240525191727.png]]
la procedura prende in input il valore della chiave da inserire e l'albero, abbiamo subito due casi:
- se la radice non è piena allora chiamiamo subito InsertNotFull
- altrimenti allochiamo nuovo spazio ovvero una nuova root vuota che punta alla vecchia root (facendo crescere l'albero) chiamiamo poi lo split (*split preventivo*) e poi viene chiamato InsertNotFull perchè a questo punto siamo certi che il nodo non sia pieno

![[Pasted image 20240525192333.png]]
a questo punto InsetNotFull che è il vero e proprio inserimento e funziona così:
- se *x* è un nodo foglia allora iteriamo un ciclo per trovare la posizione giusta ed inseriamo l'elemento
- se *x* non è ancora una foglia allora
	- scorriamo le chiavi dei vari livelli per trovare la posizione corretta dove scendere
	- passati al livello successivo effettuiamo uno split se il nodo è pieno
	- chiamiamo ricorsivamente la procedura fino a che il nodo non è una foglia

##### Correttezza e Complessità:
Terminazione: 
è facilmente verificabile dal codice

Correttezza:
l'invariante è: all'inizio di ogni esecuzione di BTreeInsertNonFull x è non pieno, e k va inserito nel sottoalbero radicato in x.

Complessità:
distinguiamo anche qui i due casi:
**Accesso a Disco:** la complessità dipende da tutta l'altezza e quindi abbiamo che $$Θ(h)$$
**CPU:** qui dipende ovviamente dall'altezza ma anche dal grado minimo di nodi e chiavi che andiamo a scorrere quindi come per l'operazione di ricerca abbiamo che $$Θ(t*h)=Θ(t*log_t(n))$$
