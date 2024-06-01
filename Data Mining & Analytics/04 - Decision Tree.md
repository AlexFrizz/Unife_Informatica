*gli appunti fanno riferimento al corso di data mining & analytics del classroom anno 22/23*


| Lezione Precedente | [[03 - Spazio delle Versioni]] |
| ------------------ | ------------------------------ |
| Lezione Successiva | [[05 - Bayesian Network]]      |

# Decision Trees e c4.5:
- tra gli sistemi più famosi che usano decision tree troviamo CLS, IDR, ASSISTANT, ID3 e quello che vedremo noi c4.5.

>Un albero decisionale è un modello di apprendimento automatico che rappresenta le relazioni tra le variabili in una struttura grafica ad albero
>- **Nodo Radice:** rappresenta l'inizio
>- **Nodi Interni:** rappresentano le domande successive
>- **Rami:** collegano i nodi e rappresentano le possibili risposte 
>- **Foglie:** rappresentano le classi finali o le decisioni prese

>L'algoritmo C4.5 costruisce un albero top-down, partendo dal nodo radice e procedendo verso le foglie, in questa fase, l'algoritmo usa due criteri:
>- **Guadagno di informazioni:** misura la riduzione dell'incertezza associata a una variabile dopo averla utilizzata per la classificazione
>- **Rapporto di Guadagno:** tiene conto anche del numero di esempi utilizzati per calcolare il guadagno di informazioni

![[Pasted image 20240312094401.png]]
*l'immagine rappresenta un training set relativo alle condizioni favorevoli per fare sport*

![[Pasted image 20240312094503.png]]
*rappresentazione di un decision tree basato sul dataset mostrato in precedenza*

### Tree Building Algorithm:
*build_tree(T)* returns a tree (T is the Training set):
- T contiene esempi dalla stessa classe
	- ritorna una foglia con l'etichetta della classe
- T contiene esempi da più di una classe
	- T è diviso in sottoinsiemi T1, T2, ... , Tn in base ai test sugli attributi
	- si chiama l'algoritmo ricorsivamente sul subset
	- ritorna un sottoalbero con la radice associata al test e figli child1, ..., childn

- La più comune tra le tecniche di ricerca è **Greedy Search:**
	*Caratteristiche*
	- i test sui nodi sono scelti irrevocabilmente, una volta assegnato non si può fare backtracking
	- scegliere un euristica.
		- Entropia
		- Indice di Gini

#### Tests on Attributes:
- Attributi discreti X con n possibili valori x1, ..., xn
	- **Uguaglianza a costante** X = const, due possibilità si o no
	- **Test di uguaglianza** X = ?, n possibili risposte
	- **Appartenenza in un set**: X∈S, due possibilità si o no
	- **Appartenenza ad una partizione del set** di {x1, ..., xn}, una risposta per set
- Attributi continui X
	- Confronto con una soglia X<=const, sue possibilità si o no

#### Termination Condition:
- c4.5 si ferma:
	- quando un set uniforme è trovato
	- quando un set vuoto è trovato
	- quando nessun test soddisfa almeno due sottoinsiemi contenenti un minimo numero di classi

#### Entropia:
>in information theory, l'entropia è la misura di incertezza (dispersione) associata ad una variabile random discreta
> ![[Pasted image 20240312101301.png]]
>![[Pasted image 20240312171829.png]]

- possiamo definire l'entropia di T come l'entropia della variabile random C
	- H(T) = H(C)
- nel caso di due classi + e - con probabilità p+ = P(+) e p- = P(-)
	![[Pasted image 20240312170916.png]]
- solo una variabile è indipendente p- = 1 - p+
	![[Pasted image 20240312171015.png]]

**L'entropia misura anche la non uniformità o l'impurità del set:**
- è minima quando il set è uniforme (contiene esempi di una sola classe)
- è massima quando è al massimo dell'impurità (contiene un numero uguale di esempi delle due classi)
![[Pasted image 20240312171545.png]]
*il grafico mostra l'entropia di un training set T*

##### Information Gain:
- **Information Gain of a Test:** il valore ottenuto dalla riduzione dell'entropia dato il partizionamento del set di esempi in funzione del test

>rappresenta il valore utilizzato per decidere quale attributo del dataset utilizzare per la suddivisione in ogni nodo interno all'albero
- test di X con n possibili risultati
- il set di esempi T è partizionato in n sottoinsiemi T1, ..., Tn
- l'entropia dopo la partizione in subset:
	![[Pasted image 20240312172550.png]]
- l'entropia decresce più uniforme diventa il subset
	![[Pasted image 20240312172803.png]]

*problems with Information gain*
- tendenza nel favorire test con più risultati
- il guadagno massimo è gain(X) = H(T)
	- con H(T) = 0

##### Normalized Gain:
- il guadagno è normalizzato rispetto all'entropia del test stesso
- la variabile random in questo caso è il valore dell'attributo X
	![[Pasted image 20240312173400.png]]
- gain ratio
	![[Pasted image 20240312173415.png]]

#### Gini Index:
>E' la misura di misura statistica della diseguaglianza nella distribuzione di una variabile di una popolazione
>- può assumere valori compresi tra 0 e 1
>- un indice di Gini elevato indica una maggiore disuguaglianza

![[Pasted image 20240312174731.png]]
*formula di gini per una singola classe*
![[Pasted image 20240312174754.png]]
*formula di gini per due classi*

- il massimo di disuguaglianza si ottiene con p+ = p- = 0.5 
	- gini(T) = 0.5
- il minimo si ottiene con p+ = 0 o p- = 0
	- gini(T) = 0
![[Pasted image 20240312175037.png]]
*il grafico mostra la rappresentazione di un indice di gini generico*

![[Pasted image 20240312175156.png]]
*possiamo generalizzare e dire che gini ha il massimo per p1=p2=pk=1/k*

- gini dopo aver partizionato X
	![[Pasted image 20240312175332.png]]


### Attributes with Unknown Value:
- assumiamo che i valori sconosciuti abbiano la stessa distribuzione P(xj, cj) 
- gli esempi con valori sconosciuti non alterano i valori di Entropia o Gini Index
- c4.5 penalizza gli attributi con valori sconosciuti moltiplicando il guadagno con la probabilità che gli attributi siano conosciuti
	![[Pasted image 20240312180055.png]]

*esempio - vedi comunque slide lezione*
- Immaginiamo di non conoscere l'esempio D6 all'attributo Outlook
- esempio D6 assegnato ai set Tsunny, Tovercast, Train con pesi 5/13, 3/13, 5/13
	![[Pasted image 20240312183220.png]]
	*nell'immagine vediamo come il subset Tsunny si comporta con un unknown value*
	![[Pasted image 20240312183419.png]]
	*calcolo dei pesi per i vari attributi in base alle classi P ed N*

*Output interpretation:*
- numeri (A/B) associati
	- A= peso totale degli esempi associati alla foglia
	- B= peso totale degli esempi associati alla foglia che sono misclassified
	
	*es* N (3.4/0.4)
	- significa che 3.4 casi sono legati alla foglia di cui 0.4 non sono associati alla classe N

#### Classification of Unseen Cases with all Values Known:
- un nuovo caso non visto *e* con tutti gli attributi conosciuti, è classificato con:
	- attraversa l'albero dalla radice seguendo i rami che corrispondono ai valori
	- supponiamo che la foglia P (A/B) sia raggiunta e classificata come P con pobabilità (A-B)/A e N con probabilità B/A

#### Classification of Unseen Cases With Unknown Values:
- T: nodo con un test su X, *e* ha X sconosciuto
- *e* è associato ad un peso *w* inizialmente settato a 1
- attraversando l'albero se l'attributo al nodo corrente è sconosciuto in *e* esploriamo tutti i subtree
- subtree Ti è espolorato assegnando *e* a Ti con un peso *w* * P(xi)
- P(xi) è stimata considerando la relativa frequenza di in T
	![[Pasted image 20240312184949.png]]
	*nella formula c=classe e=esempio l=istanza del set L*
	
![[Pasted image 20240312185057.png]]
![[Pasted image 20240312185120.png]]
*le immagini mostrano un esempio in cui vengono associati dei pesi ai valori e poi calcolate le probabilità delle classi in base a questi*

### Inductive Bias di c4.5:
>- alberi più brevi sono preferibili a quelli lunghi,
>- alberi che mettono più vicini alla radice gli attributi con l'information gain più alto sono preferibili

### Overfitting:
- un ipotesi overfitta i dati quando esiste una ipotesi più semplice, meno accurata nel training set ma più accurata nell'universo
- overfitting può accadere quando il training set contiene errori o quando è piccolo
![[Pasted image 20240312202221.png]]

**approcci per evitare condizioni di overfitting**
- fermare la crescita prima di raggiungere un albero che classifica perfettamente gli esempi del training set (**prepruning**)
- far crescere l'albero completamente e potarlo in seguito (**postpruning**)

*ma come si determina la dimensione ottimale di un albero?*
- usando un set di esempi separati per valutare l'albero (training and validation)
- usare tutti gli esempi per il training ma usare test statistici per decidere se tenere un nodo o no
- usare una misura esplicita della complessità dell'albero e gli esempi che sono l'eccezione e scegliere un albero che minimizza questa misura

#### Reduced Error Pruning:
- è un approccio *postpruning* e *train and validation set*
- dividi i dati
- mentre una potatura riduce l'accuratezza del validation set:
	- valutare l'impatto del pruning in ogni nodo del validation set
		- potare un nodo significa rimpiazzarlo con una foglia etichettata con la classe più frequente di quel nodo
	- potare il nodo che da l'accuratezza migliore nel validation set
![[Pasted image 20240312203251.png]]
*l'immagine mostra gli effetti del pruning di un albero sul grafico mostrato in precedenza*

#### SImplification of the Tree in c4.5:
- **Pessimistic Pruning:** è di tipo *postpruning* che utilizza un *test statistico* invece del validation set
	- l'albero che si ottiene è più semplice e spesso più accurato nella valutazione di nuovi casi perchè riduce l'overfitting
- **Pruning by Substitution:** una singola foglia in place di un albero o un sottoalbero in place di un albero
	- si basa sulla stima dell'error rate
