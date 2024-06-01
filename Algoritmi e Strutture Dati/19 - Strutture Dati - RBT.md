| Lezione Precedente | [[17 - Strutture Dati - Alberi]] |
| ------------------ | -------------------------------- |
| Lezione Successiva | [[20 - Strutture Dati - BT]]     |
_gli appunti fanno riferimento alla lezione 21 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Alberi Red-Black
>Sono una struttura dati astratta basata sui BST e con la caratteristica di essere *bilanciato* possiede quindi tutte le caratteristiche di un BST ma la sua altezza è *Θ(log(n))*
>ha le seguenti caratteristiche:
>- è una struttura *dinamica*
>- *basata sull'ordinamento*
>- e *sparsa*

la complessità di tutte le operazioni sono proporzionali all'altezza dell'albero

oltre a tutte le caratteristiche solite degli alberi e dei BST i nodi hanno qui anche l'attributo colore (x.color) che può assumere i valori *rosso* o *nero*

ogni nodo senza figli e il padre della radice hanno puntatori *nil* collegati ai figli virtuali che non contengono chiave e sono sempre di colore nero
![[Pasted image 20240525172646.png]]

### Proprietà
gli alberi Red Black hanno 5 proprietà fondamentali (in aggiunta a quelle dei BST) che sono vincolanti e devono essere sempre rispettate anche dopo ogni operazione
- 1) Ogni nodo è rosso o nero
- 2) La radice è nera
- 3) Ogni foglia (esterna, *nil*) è nera
- 4) se un nodo è rosso entrambi i suoi figli sono neri
- 5) per ogni nodo, tutti i percorsi semplici da lui alle sue foglie contengono lo stesso numero di nodi neri

definiamo **altezza nera** *(bh(x))* di un nodo x in T come il numero di nodi neri su qualsiasi percorso semplice da x (senza contare x) a una foglia esterna (contandola).
l'altezza nera di T è *bh(T.root)*

### Operazioni
tra le varie operazioni possibili ne vediamo due che sono la rotazione e l'inserimento

#### Rotazioni
![[Pasted image 20240525173725.png]]
le rotazioni sono un modo per sistemare l'albero dopo eventuali proprietà violate dovute ad inserimento o eliminazione, può avvenire sia verso destra che verso sinistra e preserva le proprietà di BST (ma non RBT) 

l'idea è quella ribilanciare l'albero e poi preoccuparsi dei colori, in ogni caso questa operazione ha complessità costante perchè ha solo istruzioni semplici $$Θ(1)$$
![[Pasted image 20240525174216.png]]

#### Inserimento
qui la situazione si complica, perchè con l'aggiunta di un nuovo elemento potremmo rompere il bilanciamento e le proprietà dei RBT

cominciamo col dire che ogni nodo inserito è di colore *rosso* questo perchè così facendo le uniche proprietà che verrebbero violate sarebbero la 2 e la 4 che sono anche le più semplici da sistemare

![[Pasted image 20240525174524.png]]
prima di tutto dobbiamo trovare il posto corretto dove inserire il nostro nuovo nodo, e lo facciamo allo stesso modo di BSTinsert con due aggiunte 
- il nodo viene colorato di rosso
- viene chiamata la procedura per aggiustare le proprietà violate dell'albero

![[Pasted image 20240525174806.png]]
*nell'esempio inseriamo 27 nel posto corretto ma viene violata la proprietà 4*

![[Pasted image 20240525174908.png]]
la procedura di fixup si attiva ovviamente quando il padre del nuovo nodo è rosso, altrimenti non sussiste nessun problema poi:
- chiama *fixupleft* se il padre di z (*z.p*) è attaccato al ramo sinistro del nonno (*z.p.p.left*)
- chiama *fixupright* se il padre di z è attaccato al ramo destro del nonno

![[Pasted image 20240525175359.png]]
alla chiamata di questa procedura potremmo trovarci davanti a tre situazioni (2 generalizzando):
- **Caso 1:** lo zio (y) di *z* è rosso 
- **Caso 2:** lo zio (y) di *z* è nero e figlio destro
- **Caso 3:** lo zio (y) di *z* è nero e figlio sinistro

- se è *caso 1* allora semplicemente si ricolorano i nodi nel modo corretto e si controlla che il bilanciamento rimanga invariato
![[Pasted image 20240525180107.png]]

- se è *caso 2 o caso 3* ricoloriamo ed eseguiamo una rotazione per sistemare eventuali violazioni
![[Pasted image 20240525180123.png]]


#### Correttezza e Complessità
Terminazione: 
la proprietà è rispettata perchè vi sono le apposite condizioni per la terminazione dei cicli e quindi l'uscita dal codice

Correttezza:
per questa utilizziamo il seguente invariante: 
z é rosso, se z.p è la radice, allora è nera, e se T víola qualche proprietá, allora ne víola esattamente una, che è la 2 o la 4 (se é la 2, è perché z è la radice ed è rossa, se é la 4, è perché z e z.p sono entrambi rossi).

Complessità:
La complessità come già annunciato dipende dall'altezza degli alberi e dalla forma, essendo un albero bilanciato quello dei RBT diciamo inoltre che
- la complessità di rotazioni è costante (quindi non impatta sull'inserimento)
- non vi sono chiamate ricorsive
- in ogni caso per l'inserimento tutta l'altezza dell'albero è da percorrere quindi
$$Θ(log(n))$$

### Confronto
![[Pasted image 20240525180907.png]]
