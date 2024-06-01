| Lezione Precedente | [[12 - Strutture Dati - Liste]]       |
| ------------------ | ------------------------------------- |
| Lezione Successiva | [[13 - Strutture Dati - Pile e Code]] |
_gli appunti fanno riferimento alla lezione 14 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

### Pile (o Stack):
>Una pila (o stack) è una struttura dati astratta che cresce o decresce dall'alto e che implementa la politica LIFO (Last in First out)
>*applicazioni*
>- valutazione di espressioni
>- processi di backtracking
>- eliminazione della ricorsione

*caratteristiche*
- **astratte**
- **dinamiche**
- **non basate sull'ordinamento**
- possono essere implementate sia in maniera **compatta** (array) o **sparse** (liste)
- l'accesso ai dati nel caso di queste strutture dati sono gestite da una politica di accesso

#### Implementazione:
- come abbiamo detto è possibile costruire una pila basandoci sulla struttura di un array, quindi in maniera compatta o sulle liste, in maniera sparsa

##### Pile su array:
- in questo caso interpretiamo il nostro array come uno stack
- assumiamo che sia composto da S interi 
- e che abbia i parametri *S.top* (ultima casella occupata)
- ed *S.max* (capacità massima di S)
- le operazioni di inserimento ed eliminazione sono *Push* e *Pop*
![[Pasted image 20240318092047.png]]
*nell'immagine vediamo lo pseudocodice dell'implementazione di una pila*

##### Pile su liste:
- le pile possono essere implementate anche utilizzando la struttura di una lista
- considerando ovviamente che avendo una struttura sparsa e non compatta le operazioni di inserimento e cancellazione vanno gestite con i puntatori
![[Pasted image 20240318094521.png]]
*pseudo codice di una pila implementata con una struttura a lista*
### Code (o Queue):
>Una coda è una struttura dati elementare che implementa una politica FIFO (first in first out)
>*applicazioni*
>- playlist 
>- processi di viste, grafi

*caratteristiche*
- permette di accedere ai dati tramite inserimento ed eliminazione (enqueue) e (dequeue)

#### Implementazione:
- anche in questo caso è possibile utilizzare la struttura di un array per la realizzazione di una coda
##### Code su array:
- in questo caso assumiamo che Q sia una coda
- con parametri *Q.head* e *Q.tail* entrambi naturali e inizializzati a 1
- siccome *Q.head = Q.tail* quando la pila è vuota e piena
- aggiungiamo una variabile *dim* che inizialmente è 0 (pila vuota)
![[Pasted image 20240318093725.png]]
*l'immagine mostra lo pseudocodice di un implementazione di una coda con inserimento e cancellazione*

##### Code su liste:
- anche in questo caso possiamo implementare la struttura di una coda tramite liste e puntatori
- in questo caso avremmo due puntatori uno alla testa e uno alla coda della struttura
![[Pasted image 20240318094755.png]]
*nell'immagine uno pseudocodice di una coda con l'utilizzo di liste e puntatori*


### Conclusioni:
- qualsiasi sia il caso di implementazione di liste e code è indifferente l'utilizzo di array o liste come struttura, tranne per tutte le questioni relative alla memoria
- in tutti e quattro i casi gli algoritmi si comportano con complessità *Θ(1)* non avendo cicli e avendo solo operazioni elementari
- ovviamente è facilmente verificabile come anche le proprietà *correttezza, completezza e terminazione* siano fornite

