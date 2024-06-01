*gli appunti fanno riferimento alle slide delle lezioni 5-6-7 del classroom di data mining & analytics del 22/23*

| Lezione Precedente | [[04 - Decision Tree]] |
| ------------------ | ---------------------- |
| Lezione successiva | [[06 -kNN]]            |
## Domain Modeling:
- vogliamo utilizzare un insieme di variabili random per descrivere un dominio di interesse
![[Pasted image 20240314094145.png]]
*l'immagine mostra un esempio di modellazione del dominio*

### Inferenza:
>è un processo attraverso il quale mediante delle premesse si deriva una proposizione
>si può considerare anche come un processo deduttivo

*nel nostro esempio*
- quale è la probabilità di un furto attraverso la porta? P(b2)
- oppure quale è la probabilità di un furto attraverso la porta dato che il vicino ha chiamato (probabiilità condizionata) P(b2|n2)
- oppure probabilità di un furto attraverso la porta e dell'allarme dato che c'è stato un terremoto moderato e il vicino ha chiamato? P(a2,b2|n2,e2)

*possiamo avere più categorie di problemi*
in base alle operazioni che vogliamo compiere
- Diagnosis: (cause|symptom)=?
- Prediction: (symptom|cause)=?

*possiamo ottenere*
- Classification: argmax P(class|data)

*in generale*
vogliamo calcolare la probabilità condizionata P(q|e)
- di una query *q* (istanza di un set di variabili Q)
- data l'evidenza *e* (istanza di un set di variabili E)

#### Joint Probability Distribution:
la jpd di un set di variabili U è data da P(u) di tutti i valori di *u*
- U = {E, B, A, N}
- abbiamo la jpd se conosciamo P(u) = P(e, b, a, n) per tutti i possibili valori di e, b, a, n
- per insiemi molto grandi potremmo avere dei problemi nel rappresentarli abbiamo quindi le seguenti notazioni
![[Pasted image 20240314095539.png]]
*vediamo qui la formula della probabilità congiunta*
![[Pasted image 20240314100007.png]]
*esempio di indipendenza condizionata*
![[Pasted image 20240314100523.png]]
*pi-greco rappresenta tutti i valori di xi successivi a xi*
![[Pasted image 20240314100546.png]]
*esempio di chain rule data la notazione pi-greco*

#### Rappresentazione Grafica:
- possiamo rappresentare l'indipendenza condizionata usando una forma grafica a nodi
- il grafo è aciclico
![[Pasted image 20240314101004.png]]
*esempio di grafico*

![[Pasted image 20240314101240.png]]
*esempio di tabelle di probabilità condizionata*

### Bayesian Network (BN):
> Una rete bayesiana è un modello grafico che rappresenta le relazioni di probabilità tra le variabili

- è data da una coppia (G, Θ) dove
	- G è un grafo diretto aciclico (DAG) (V,E) dove
		- V è un set di vertici {X1, ..., Xn}
		- E è un set di angoli (Xi, Xj)
		- < X1, ..., Xn > è un ordinamento di G (Xi, Xj) E-> i < j
	- Θ è un set di tabelle di probabilità condizionata (cpts)


#### How to Build a Bayesian Network:
- scegliamo un ordinamento per X1, ..., Xn per le variabili
- facciamo un ciclo da i=1 a n
	- aggiungiamo Xi alla rete
	- aggiungiamo i genitori, ovvero quali sono gli elmenti {X1, ... Xi-1} tale che abbiamo indipendenza condizionata per Xi
	- assegnamo un valore a P(xi, pigrecoi) per tutti i valori di x i e pigrecoi

- solitamente si considera di aggiungere una variabile X come figlia di Y se Y è una causa diretta di X
- **correlazione e causalità sono legate ma non sono la stessa cosa, può esserci correlazione anche senza causalità, ma se c'è causalità c'è anche correlazione**
- *è fondamentale per la costruzione di una rete consultare un esperto in base al campo di applicazione*
##### Indipendenza:
- per determinare se due variabili in una rete bayesiana sono condizionalmente indipendenti si utilizza il criterio *d-separation*
> X e Y sono d-separate da un insieme di variabili evidenza Z se ogni percorso non direzionato da X e Y è bloccato, dove un percorso è bloccato se si verificano una o più di queste condizioni:

- esiste una variabile V nel percorso (in the evidence set Z) con questa forma: (tail-to-tail)
	![[Pasted image 20240314175353.png]]
- esiste una variabile V nel percorso (in the evidence set Z) con questa forma: (tail-to-head)
	![[Pasted image 20240314175434.png]]
- esiste una variabile V nel percorso (NOT in the evidence set Z e nessuno dei suoi discendenti) con questa forma: (head-to-head)
	![[Pasted image 20240314175628.png]]

#### Markov Blanket:
>il markov blanket di una variabile è l'insieme di variabili S tale che rendono indipendente V da tutte le altre variabili
>I<{V}, S, U-S-{V}>

![[Pasted image 20240314182553.png]]
*nell'immagine vediamo una rappresentazione di quello che è il markov blanket*
- all'interno del cerchio ci sono tutte le variabili S che rendono A indipendente, e sono;
	- i suoi padri
	- i suoi figli 
	- i padri dei suoi figli
- la proprietà si può verificare tramite *d-separation*

### Inferenza nelle Reti Bayesiane
esistono vari algoritmi per fare inferenza nelle reti bayesiane, in generale possiamo dividerli in tre categorie:
- **exact** method for polytrees
	- belief propagation
- **exact** method for general networks
	- junction tree
	- variable elimination
- **approximate** methods for general network
	- stochastic simulation
	- loopy belied propagation
	- variational methods

>la complessità dell'inferenza è #P-Complete, che sono più difficili di problemi NP-Complete, data la loro natura infatti la soluzione di #P-Complete è il numero delle soluzioni NP-Complete

#### Polytrees:
> un polytree è un grafo aciclico direzionato in cui non ci sono due nodi che hanno più di un percorso 
> possiamo anche dire che se togliamo le direzioni alle frecce non devono comparire cicli

![[Pasted image 20240314184432.png]]
*la prima immagine rappresenta un polytree, mentre la seconda no*

##### Belief Propagation - [[BP - Esempio]]:
- mostreremo questo algoritmo nei **factor graph** ossia un grafo bipartito (V, F, E) dove vertici V indicano le variabili, i vertici F le famiglie (factors) e angoli non direzionati E connessi a V ed F
- una famiglia può essere considerata come un nodo più i suoi genitori, alla quale può essere associata una tabella condizionata
![[Pasted image 20240314184936.png]]
*nell'immagine sono rappresentati i fattori di un factor graph (in centro) e una tabella di probabiltà condizionata*

>la belief propagation è un algoritmo che prevede lo scambio di messaggi tra i nodi fino a quando si stabilizzano e non cambiano più (convergenza)

###### Messages:
- i messaggi da una variabile X a un fattore f vicino è:
	![[Pasted image 20240314190415.png]]
	*dove nb(X) è il set di neighbor di X*
- i messaggi da un fattore ad una variabile è:
	![[Pasted image 20240314190459.png]]
	*dove nb(X) è il set di argomenti di f e la somma è su tutti questi tranne X*

###### Algorithm:
- si inizializzano tutti i messaggi ad 1/n o random
- loop
	- seleziono un arco
	- calcolo il valore del messaggio sull'arco
- fino a quando i messaggi non cambiano più
- se la rete è un polytree, questo algoritmo converge

*quale strategia scelgo per aggiornare l'arco?*
###### Message schedules:
- l'ordine in cui i messaggi sono aggiornati
- **Asynchronous Schedules:** i messaggi sono aggiornati sequenzialmente un arco alla volta (fornisce le migliori performance)
- **Synchronous Schedules:** tutti i messaggi sono aggiornati in parallelo

#### General Network:
*abbiamo visto come funziona un algoritmo di belief propagation in un polytree, ma se la forma della rete fosse diversa?*

- **conditioning:** spezzo i cicli impostando un variabile ai suoi due valori e poi faccio la belief propagation su entrambe le reti e poi sommo i valori
	![[Pasted image 20240315092218.png]]
- **clustering:** raggruppo le variabili in modo da far risultare la rete come un polytree e usare belief propagation
	![[Pasted image 20240315092140.png]]

##### Variable Elimination - [[VE - Esempio]]:
> E' un algoritmo di tipo exact inference per reti generali utilizzato in modelli grafici probabilistici come reti bayesiane, markov random fields per trovare la distribuzione di probabilità di un sottoinsieme di variabili

- esegue due operazioni sui fattori, somma e prodotto
- function sum-out (lista di fattori F, variabile z)
	- rimuove da F tutti i fattori f1, ..., fk che contengono z
	- somma il nuovo fattore a F
		![[Pasted image 20240315095021.png]]
	- return F

###### Algorithm:
- l'algoritmo VE utilizza le variabili (F, q, e, O)
	- F insieme di fattori, q  query, e evidence, O ordine delle variabili in U\ Q\ E
- si impostano le variabili osservate E in tutti i fattori al loro valore osservato e (si rimuovono le variabili dal fattore)
- fintanto che O non è vuoto
	- rimuovo la prima variabile v da O
	- F=sum-out(F, v)
- imposto h= al prodotto di tutti i fattori in F
- return h(q)/SUMh(Q)

#### Approximate Methods:
- **Stochastic Simulation:** 
	- genera N samples (assegnamento di valori a tutte le variabili del dominio) da BN
	- conta Ne samples che soddisfa *e*, Nqe che soddisfa *q, e* (query, evidence)
	- P(q|e) = Nqe/Ne
- **Loopy belief propagation:**
	- bp in reti con cicli
	- gli esperimenti mostrano che converge anche in reti con cicli, spesso con soluzioni di buona qualità

##### Stochastic Simulation:
//appunti...

### Problems in Building BN:
>per costruire una rete bayesiana è fondamentale avere un ottima conoscenza del dominio di applicazione (indipendenza condizionale), per fare questo spesso si consulta un esperto del dominio
- di solito gli esseri umani usano l'informazione causale, cioè intuizioni sulle relazioni tra le variabili ad esempio se una variabile ne causa un altra allora la prima sarà il genitore
- inoltre non è banale assegnare un numero alla tabella di probabilità condizionale non conoscendo il dominio

- abbiamo però un set di osservazioni D = {u1, ..., uN}
- uj è un assegnazione a tutte le variabili U = {X1, ..., Xn}
- per fare inferenza sui parametri e sulle strutture da D utilizziamo i learning
#### Learning:
- vogliamo trovare una BN su U tale che la probabilità dei dati P(D) sia massimizzata
- P(D) è anche chiamata *verosimiglianza* dei dati
- assumiamo che tutti i campioni siano *indipendenti e ugualmente distribuiti iid*
	![[Pasted image 20240315180408.png]]
- spesso ci troviamo ad utilizzare la funzione logaritmo (*log likelihood*)
	![[Pasted image 20240315180451.png]]

*quindi*
- Task:
	- calcolare i parametri data una struttura fissa o
	- trovare struttura e parametri
- Proprietà dei dati:
	- dati completi: in ogni vettore *uj* i valori di tutte le variabili sono osservati
	- dati incompleti

#### Parameter Learning from Complete Data:
 - parametri da apprendere:
	 ![[Pasted image 20240315180912.png]]
 - i valori dei parametri che massimizzano la verosimiglianza possono essere calcolati in forma chiusa (con una formula senza dover fare più passaggi sui dati)

*parametri di massima verosimiglianza*
- parametri dati dalla frequenza relativa 
- se Ny è il numero di vettori di D dove Y=y
	![[Pasted image 20240315181537.png]]
- contare: per ogni i, per ogni pigrecoi, dobbiamo raccogliere
	![[Pasted image 20240315181709.png]]
- dove vi è il numero di valori di Xi

#### Structure Learning from Complete Data:
- compie una ricerca locale nello spazio di possibili strutture (G)
- algoritmo HGC
	- comincia con una struttura bestG' (potrebbe essere anche vuota)
	- ripete
		- BestG = BestG'
		- raffinamento (modifiche locali) Ref = {G|G (insieme dei grafi G) ottenuto da BestG' aggiungendo, cancellando o invertendo un arco}
		- punteggio: BestG' = argmaxG' {score(G)|G ∈ Ref}
	- fintanto che score(BestG')-score(BestG) > 0

*calcolo dello score*
![[Pasted image 20240315185034.png]]
*probabilità dei dati D e dei parametri Theta data la struttura G*
dove
![[Pasted image 20240315185053.png]]

- se assumiamo che la distribuzione di probabilità (densità) sui parametri sia Dirichet, allora lo score è chiamato BD(for Bayesian Dirichlet) 
	![[Pasted image 20240315185943.png]]
- dove BDi(G) dipende solo da Ci e C'i, il conteggio per la famiglia di Xi
	![[Pasted image 20240315190040.png]]
	*vettore Ci dei conteggi calcolato dai dati, e vettore C'i dato a priori*

![[Pasted image 20240315190302.png]]
*la funzione gamma è un estensione della funzione fattoriale con i suoi argomenti abbassati di 1, per numeri reali e numeri complessi* 
![[Pasted image 20240315190658.png]]
![[Pasted image 20240315190711.png]]

- lo score BD(G) è scomponibile
	- può essere calcolato indipendentemente su ogni famiglia
- BD(G) può essere calcolato rapidamente da BD(BestG') cambiando solamente lo score delle famiglie interessate 

