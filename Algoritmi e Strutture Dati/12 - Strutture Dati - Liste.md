| Lezione Precedente | [[09 - QuickSort]] |
| ------------------ | ------------------ |
| Lezione Successiva |                    |
_gli appunti fanno riferimento alla lezione 13 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

### Strutture Dati:
*caratteristiche*
- **statica o dinamica**: sono strutture pensate per aggiungere o togliere elementi durante l'esecuzione (nel caso di dinamica), nel caso non si possa fare sono strutture (statiche)
	- array: statici
- **compatta o sparsa**: tutte le posizioni fisiche in memoria sono vicine (nel caso di compatta), nel caso in cui siano distanti siamo nel caso di struttura sparsa 
	- array: compatti
	- le strutture dinamiche sono tipicamente sparse
- **basata o non basata sull'ordinamento**: gli elementi sono disposti in maniera dipendente dal valore delle chiavi (caso basato sull'ordinamento), altrimenti no

#### Chiavi e Dati Satellite:
- **Chiavi:** rappresentano gli indici o gli id dei dati
- **Dati Satellite**: Rappresentano tutta una serie di dati associati a quella determinata chiave 
- *es.* il numero di matricola di uno studente potrebbe essere la chiave, di conseguenza tutte le informazioni associate ad esso come *nome, cognome, indirizzo, ecc.* rappresentano i dati satellite

#### Concrete & Astratte:
*un altro modo di distinguere le strutture dati è dividendole in concrete e astratte*
- **Concrete**: (o fondamentali), sono quelle che vengono offerte di base dal linguaggio
- **Astratte**: vengono create sopra alle strutture concrete, nascondendone anche l'implementazione, vengono utilizzate per compiti specifici
*spesso la distinzione tra queste non è netta (come nel caso delle liste)*


### Puntatori e Lista:

#### Lista:
*caratteristiche*
- **dinamica**
- **non basata sull'ordinamento**
- **sparsa**
*associamo le operazioni*
- **inserimento**
- **cancellazione**
- **ricerca**
*considerazioni*
- possiamo considerare una lista come una versione dinamica di un array
- l'operazione di ordinamento delle chiavi normalmente avviene attraverso la copiatura degli elementi su un array

#### Puntatore:
*caratteristiche*
- è un tipo di dato fondamentale, un oggetto che contiene un indirizzo
- viene associato al tipo di dato puntato 
- *es.* un puntatore ad un intero è diverso da un puntatore ad un altro tipo di variabili 

> nella costruzione di liste, creiamo un oggetto complesso che contiene (nel caso generale di liste concatenate):
> - una chiave 
> - un puntatore al prossimo elemento (successore)
> - un puntatore all'elemento precedente (predecessore)
> diventa un tipo di dato ricorsivo perchè nella dichiarazione del tipo di dato assegnamo quel tipo a variabili all'interno della struttura 

> ai fini didattici distinguiamo il contenuto dal contenitore, quindi avremo:
> - un oggetto di tipo lista, come una struttura, che contiene i dati dei puntatori e chiavi
> - una struttura locale che descrive la lista nella sua interezza coma ad esempio il numero degli elementi l'indirizzo del primo elemento *ecc.*

### Operazioni su Liste:
*suddividiamo le operazioni in Inserimento, Ricerca e Cancellazione*

#### Inserimento:

![[Pasted image 20240305115404.png]]
*fig1: rappresentazione concettuale di lista e puntatore*

![[Pasted image 20240305115453.png|400]]
*fig2: pseudocodice per creare collegamento tra gli elementi della lista (inserimento in testa alla lista)*

#### Ricerca:

![[Pasted image 20240305120716.png|500]]
*fig3: pseudocodice per la ricerca di elementi all'interno della lista per chiave*

#### Delete: 

 ![[Pasted image 20240305121326.png|500]]
 *fig4: rappresentazione grafica del delete di un elemento dalla lista*
 
![[Pasted image 20240305121455.png|400]]
*fig5: pseudocodice per l'eliminazione di un elemento della lista assumendo di conoscerne l'indirizzo x*


#### Correttezza e Complessità delle operazioni su Lista: 
come abbiamo visto la correttezza di queste operazioni è immediata da dimostrare, diremo quindi che sono tutte:
- **Terminanti**
- **Corrette**
- **Complete**
per quanto riguarda la complessità delle operazioni diremo:
- **ListInsert:** avendo solo operazioni semplici e nessun ciclo o chiamata ricorsiva la complessità corrisponde a:
$$O(1)$$
- **ListSearch:** i questo caso è presente un ciclo while che scorre dal primo, tutti gli elementi fino al completamento della ricerca, corrisponde a: (nel caso peggiore)
$$O(n)$$
- **ListDelete:** come nel caso di ListInsert non abbiamo solo operazioni semplici quindi la complessità corrisponde a costante:
$$O(1)$$

### Conclusioni:
per concludere occorre valutare in base alle necessità se utilizzare strutture dati come ad esempio Array o Liste