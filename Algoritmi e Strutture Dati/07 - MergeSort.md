| Lezione Precedente | [[06 - Esercitazione 1]] |
| --- | --- |
| Lezione Successiva | [[08 - Laboratorio 0]] |
_gli appunti fanno riferimento alla lezione 7 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

#### Algoritmi di Ordinamento:
gli algoritmi di ordinamento sono tra i più semplici ed allo stesso tempo tra i più importanti dell'algoritmica, alcuni degli usi diretti:
- **ricerca di un elemento:** molto più rapida se effettuata su un insieme ordinato
- **coppia di elementi più vicina:** coppia di elementi a minima distanza di un insieme
- **unicità degli elementi:** stabilire se un elemento è unico in quell'insieme
- **distribuzione di frequenze:** quante volte appare un elemento
- **shuffling (disordinamento):** ordinare in modo casuale un insieme

possiamo categorizzare gli algoritmi di ordinamento in 3 categorie (notazione informale di prof. Guido Sciavicco):
- elementari basati sul confronto
- non elementari basati sul confronto
- non basati sul confronto

*per elementare intendiamo che ogni elemento viene spostato verso il/al posto giusto separatamente dagli altri, in maniera iterativa o ricorsiva*

#### MergeSort:
*MergeSort è un algoritmo di ordinamento non elementare*
MergeSort è composto da due procedure:
- **Merge**: costruisce una sequenza ordinata partendo da due sequenze ordinate
	- prende in input l'array *A*, l'indice minimo *p* l'indice massimo *r* e l'indice medio *q*
	- *n1* ed *n2* rappresentano rispettivamente le posizioni del primo e del secondo array ordinato
	- l'istruzione *let* alloca le posizioni *[1, ..., n1]* ad *L* e le posizioni *[1, ..., n2]* ad *R*
	- i due cicli *for* copiano gli elementi dei due array su *L* ed *R*
	- poi abbiamo un ciclo *for* che scorre tutti gli elementi dell'array unito e all'interno due controlli che tengono conto del caso nel cui un array arrivi alla fine prima dell'altro
	- il successivo controllo confronta i due elementi e chiama le procedure adeguate per la copia
- **MergeSort**: implementazione stessa dell'algoritmo, l'obbiettivo di MergeSort è quello di chiamarsi ricorsivamente per dividere l'array di partenza in sottoarray di dimensione 1 che per definizione sono già ordinati e poi riordinarli con merge
 ![[Pasted image 20240213105018.png]]
 *fig1: in questo caso vediamo l'implementazione della procedura Merge*
 
**Considerazioni su Merge e MergeSort:**
- la scelta di usare due oggetti (array) esterni *L* ed *R* è molto conveniente per poter usare *A* come oggetto di arrivo ma questo significa che Merge non è in place:
	- cioè tanto maggiore sarà l'input e maggiore sarà la quantità di spazio utilizzata da *L* ed *R* che pertanto non è costante 
	- questa caratteristica ovviamente viene ereditata da MergeSort perchè implementa Merge

#### Correttezza di Merge:
- **Terminazione:** all'interno di merge abbiamo più cicli *for* le cui variabili incrementali non vengono manipolate se non per per la gestione del ciclo inoltre assumiamo che partano sempre più piccole della condizione di terminazione, questo ci assicura l'uscita dal ciclo
- **Correttezza:** il terzo ciclo *for* rappresenta il nostro invariante (vera prima della prima iterazione, dopo la prima e dopo l'ultima iterazione)
	- *A[p, ..., k-1]* contiene i *k - p* elementi più piccoli di *L[1, ..., n1]* e  di *R[1, ..., n2]* ordinati
	- inoltre *L[i]* (se *i<=n1*) e *R[j]* (se *j<=n2*) sono i più piccoli elementi di *L* ed *R* non ancora ricopiati in *A*
	- *volgarmente, la parte di A che ho già ricopiato è sicuramente ordinata*
- **Completezza:** essendo Merge un algoritmo noto diamo per assodata la completezza 

#### Complessità di Merge:
ignoriamo per comodità tutte le costanti quindi consideriamo solo i cicli:
- i primi due cicli si eseguono per un numero di volte rispettivamente pari ad *n1* ed *n2* o se si vuole anche pari entrambi ad *n/2* otteniamo quindi che la complessità del primo ciclo più il secondo è *Θ(n)*
- il terzo ciclo invece si esegue per in numero di volte pari a tutta la dimensione dell'array quindi *n* avendo all'interno del ciclo solo passi costanti anche in questo caso ottengo *Θ(n)*

*abbiamo quindi una complessità Θ(n) in tutti i casi e la complessità è lineare* 

#### Correttezza di MergeSort:
![[Pasted image 20240213121133.png]]
*l'algoritmo MergeSort prevede due chiamate ricorsive per dividere gli array ogni volta a metà, quando gli array sono di dimensioni 1 il controllo (p<q) ovviamente falso fa uscire dalla chiamata ricorsiva e restituisce il controllo al chiamante che eseguirà Merge per riunire gli elementi in maniera ordinata*
- **Terminazione:** la terminazione è ben definita perchè la chiamata ricorsiva viene effettuata solo quando *p<r* ma quando gli array saranno di dimensione 1 avremo che *p>=r* e quindi non verranno più effettuate chiamate ricorsive
- **Correttezza:** abbiamo due casi ben definiti:
	- l'algoritmo è corretto perchè Merge è corretto
	- usiamo l'invariante induttiva perchè non avendo cicli definiamo la correttezza per una proprietà di una chiamata ricorsiva di MergeSort, dopo ogni chiamata infatti avremo che *A[p, ..., q]* e *A[p, ..., q+1]* sono ordinati perchè vengono passati a Merge
- **Completezza:** essendo MergeSort un algoritmo noto diamo la propeirtà della completezza per assodata

#### Complessità di MergeSort:
avendo già calcolato la complessità di Merge, partiamo avvantaggiati sapendo che questa è *Θ(n)*
- il numero di chiamate ricorsive quindi è 2
- la complessità di queste chiamate è *T(n/2)* perchè vengono chiamate sulla metà degli elementi
abbiamo quindi che la complessità di MergSort ha questa forma:
![[Pasted image 20240213124307.png]]
è inoltre facilmente risolvibile con il master theorem (caso 2) e si ottiene:
![[Pasted image 20240213124354.png]]

##### Conclusioni:
>è importante sottolineare che MergeSort non dipende in nessun modo da come è ordinato l'array iniziale, (al contrario ad esempio di InsertionSort) questo perchè indipendentemente dall'ordine l'array viene diviso in *n* elementi e poi riordinato; quindi come si può facilmente intuire non esistono casi migliori, medi o peggiori
- possiamo inoltre facilmente affermare che MergeSort è più efficiente di InsertionSort abbiamo infatti che *Θ(n * log(n))* = *o(n^2)* 
- MergeSort è un chiaro esempio della strategia *divide and conquer* esistono naturalmente altre strategie come *programmazione dinamica* o *greedy* per citarne alcune
- avremo quindi una rappresentazione simile a questa nel grafico
![[Pasted image 20240213125220.png]]
