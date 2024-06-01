*gli appunti fanno riferimento alle slide della lezione 8 del classroom di data mining & analytics del 22/23*

| Lezione Precedente | [[05 - Bayesian Network]] |
| ------------------ | ------------------------- |
| Lezione successiva | [[07 - Perceptron]]       |

## Apprendimento Basato sulle Istanze:
- **Apprendimento:** semplice memorizzazione di tutti gli esempi <xi, f(xi)>
- **Classificazione nuova istanza xj:** reperimento degli esempi più simili e classificazione sulla base di questi

### K-Nearest Neighbour:
>kNN è un algoritmo per il riconoscimento di pattern per la classificazione di dati che si basa sulle caratteristiche degli oggetti vicini a quello considerato per la classificazione
- si considerano i *k* più vicini ad *xq* 
	- se la funzione *f* è discreta allora si restituisce il valore *v* di *f* più frequente tra *k* esempi
	- ogni valore *xi* assegna un voto al valore *v* per il quale *f(xi)=v*
	![[Pasted image 20240319200914.png]]
	*l'immagine mostra la formula relativa al caso discreto*
	
	- se la funzione *f* è continua si restituisce la media tra i valori di *f* per i *k* esempi
	![[Pasted image 20240319201047.png]]
	*l'immagine mostra la formula relativa al caso continuo*

**vantaggi:**
- non c'è bisogno di apportare una descrizione globale della funzione *f*, si generano solo approssimazioni locali
**svantaggi:**
- alto tempo per la classificazione
- tutti gli attributi sono considerati, se il concetto target dipende solo da pochi attributi, gli esempi effettivamente più simili possono essere a grande distanza

#### Caratteristiche:
- assumiamo che le istanze siano punti dell'insieme R
**distanza euclidea p=2:**
	![[Pasted image 20240319201944.png]]
**distanza di minkowski di ordine p:**
	![[Pasted image 20240319202039.png]]
**distanza di manhattan p=1:**
	![[Pasted image 20240319202117.png]]
**distanza di chebyshev p=infinito:**
	![[Pasted image 20240319202212.png]]

*problema*
- gli attributi possono avere diverse scale, quindi la loro importanza nella misura della distanza può essere diversa
- per risolvere questo problema cambiamo la scala di ciascun attributo in modo che i valori siano compresi tra 0 e 1
	- siano *a_min* e *a_max* rispettivamente i valori minimo e massimo dell'attributo *ai* nel training set
	- il valore *a'(x)* scalato tra 0 e 1 è dato da
	![[Pasted image 20240319202656.png]]

- se gli attributi sono nominali ad esempio *alto*, *medio* e *basso* non potendo definire una distanza nel caso più semplice si assegna 0 se i valori sono identici e 1 se i valori sono distanti (diversi)
- in altri casi potremmo avere distanze definite ad hoc basate su altre informazioni
	- es. potremmo fornire una distanza diversa alle coppie *basso, medio* e la coppia *basso, alto*

#### Osservazioni:
- k-NN è robusto agli errori perchè si prende la media di *k* esempi
- **bias induttivo:** assunzione che la classificazione di una istanza *xq* sia simile a quella dei suoi *k* vicini

##### Curse of dimensionality:
- k-NN considera tutti gli attributi degli esempi, in contrasto con altri algoritmi che selezionano solo gli attributi più rilevanti

*soluzione*
- allunga ogni asse di un peso *wj*
- *w1, ..., wn* sono scelti mediante cross-validation: un sottoinsieme degli esempi è scelto come insieme di training, i pesi sono scelti in modo da minimizzare l'errore nel classificare i rimanenti esempi
- questo processo va ripetuto più volte per affinare i pesi
- alcuni pesi possono essere impostati a 0

