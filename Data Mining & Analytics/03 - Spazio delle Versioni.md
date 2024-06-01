*gli appunti sono relativi alle lezioni del classroom 22/23 del corso di Data Mining & Analytics*

| Lezione Precedente | [[02 - Metriche di Performance e Probabilità]] |
| ------------------ | ---------------------------------------------- |
| Lezione Successiva | [[04 - Decision Tree]]                         |

## Tecniche di Apprendimento:
- Apprendimento Attributo Valore:
	- ipotesi congiuntive
	- alberi di decisione
	- regole di produzione
	- medodi basati sulle istanze
	- reti bayesiane
	- reti neurali
- Apprendimento del Primo Ordine:
	- programmazione logica induttiva
	- apprendimento statistico relazionale

### Apprendimento di ipotesi congiuntive:
- *es.* vogliamo apprendere il concetto target "giornate in cui ad Aldo piace fare sport"
![[Pasted image 20240311180046.png]]
*giornate delle quali si conosce se ad Aldo è piaciuto o meno fare sport*

#### Rappresentazione delle Ipotesi:
>Un ipotesi *h* è una congiunzione di vincoli sugli attributi dell'istanza (ipotesi congiuntive)
- Ogni vincolo può essere:
	- un valore specifico (Water = Warm)
	- un valore qualunque (Water = ?)
	- nessun valore (Water =∅)
![[Pasted image 20240311180426.png]]
- possiamo scriverlo come  h=< ?, cold, high, ?, ?, ? >
- il concetto target è *c: X -> {0,1}
	- *x(x)=1 if enjoysport=yes*
	- *x(x)=0 if enjoysport=no*
>il problema di apprendimento consiste nel determinare una ipotesi *h* che da lo stesso risultato del concetto target *c* sull'intero insieme di istanze *X*

>Ogni ipotesi trovata che approssima la funzione target correttamente su un insieme di esempi sufficientemente grande approssimerà bene la funzione target anche sugli esempi non osservati

#### Terminologia:
- se un istanza *x* soddisfa una ipotesi *h* si dice che **copre** l'esempio *x*
	- si scrive come *h(x) = 1* altrimenti *h(x) = 0*
- un ipotesi è **completa** se copre tutti gli esempi positivi (quelli con *c(x)=1*)
- un ipotesi è **consistente** se non copre nessun esempio negativo (quelli con *x(x)=0*)
- un ipotesi è **coerente** con un esempio < x, c(x) > se *h(x) = c(x)*
- un ipotesi è **coerente con un training set D** se è coerente con ciascun esempio x ∈ D
	- equivalente ad un ipotesi completa e consistente

- lo scopo della ricerca è di trovare l'ipotesi che meglio copre gli esempi del training set
- lo spazio delle ipotesi è implicitamente definito dalla rappresentazione delle ipotesi

#### Ordinamento da Generale a Specifico:
*è ovviamente impensabile di andare a ricercare i target facendo generate-and-test di tutte le ipotesi in H, basti pensare a situazioni in cui il dataset è molto grande*
- la soluzione è quella di porre due "limiti al dataset"
- ipotesi generale (*hg*) e ipotesi specifica (*hs*) all'interno delle quali vi sono una serie di ipotesi più generali di *hs* ma più specifiche di *hg* 

#### Algoritmo Find-S:
- inizializza *h* alla più specifica delle ipotesi in H
- per ciascun esempio positivo *x*
	- per ciascun vincolo *Vi* su un attributo *ai* in *h*
		- se il vincolo *Vi* è soddisfatto da *x* allora non fare niente
		- altrimenti sostituisci *Vi* in *h* con il prossimo vincolo più generale che è soddisfatto da *x* 
- restituisci l'ipotesi *h*
- find-s trova sempre una sola soluzione 
![[Pasted image 20240311183400.png]]
*l'immagine rappresenta un esempio di applicazione di find-s*

*considerazioni*
- *l'algoritmo find-s assume di base:*
	- il concetto target *c* è in H
	- nessun errore nei dati di training
- *h* è l'unica ipotesi più specifica in H che copre tutti gli esempi positivi
- quindi *c >= h*
- ma *c* non sarà mai soddisfatto da un esempio negativo 
- quindi nemmeno *h* lo sarà

*limitazioni*
- non si può dire se il learner ha trovato il concetto target corretto
	- *ha trovato l'unica ipotesi H coerente con i dati oppure ci sono altre ipotesi coerenti?*
- trova un ipotesi massimamente specifica
	- *perchè dovremmo preferire questa ipotesi rispetto ad esempio alla più generale?*

### Spazio delle Versioni (Version Space):
>Lo spazio delle versioni VS rispetto allo spazio delle ipotesi H e al training set D è il sottoinsieme delle ipotesi in H coerenti con gli esempi D
>Può essere rappresentato dai suoi membri più generali e specifici

- **Confine Generale:** il confine G rispetto allo spazio delle ipotesi H e ai dati di training D, è l'insieme dei membri di H massimamente generali che sono coerenti con D
- **Confine Specifico:** il confine S rispetto allo spazio delle ipotesi H e ai dati di training D, è l'insieme dei membri di H minimamente generali (ovvero massimamente specifici) che sono coerenti con D
#### Teorema di rappresentazione dello spazio delle versioni:
> sia X un insieme di istanze e H un insieme di ipotesi booleane definite su X
> sia c: X->{0,1} un concetto target definito su X
> sia D un insieme di esempi di training {< x, c(x) >}
> Per ogni X, H, c e D:
> ![[Pasted image 20240311201641.png]]

*una serie di algoritmi di selezione di ipotesi per raggiungere il target*
#### Algoritmo List-then-eliminate:
- VS <- lista contenente ogni ipotesi in H
- per ciascun esempio < x, c(x) > rimuovi da VS ogni ipotesi h per la quale h(x)!=c(x)
- restituisci la lista delle ipotesi in VS
##### Vantaggi e Svantaggi:
- è garantito che trovi tutte le ipotesi coerenti con i dati di training
- può trovare inconsistenze nei dati di training
- enumerazione esaustiva di tutte le ipotesi
	- possibile solo per spazi finiti H
	- non realistica per spazi H grandi

#### Algoritmo Candidate-Elimination:
- Per ciascun esempio di training *d* fai:
- se d è un esempio positivo
	- togli da G ogni ipotesi incoerente con d
	- UPDATE-S routine: per ciascuna ipotesi s in S che che non è coerente con d
		- togli s da S
		- aggiungi ad S tutte le minime generalizzazioni h di s tali che
			- h è coerente con d
			- alcuni membri di G sono più generali di h
		- Rimuovi da S ogni ipotesi che è più generale di un'altra ipotesi in S
- se d è un esempio negativo
	- togli da S ogni ipotesi incoerente con d
	-  UPDATE-G routine: per ciascuna ipotesi g in G che non è coerente con d
		- togli g da G
		- aggiungi a G tutte le minime specializzazioni h di g tali che 
			- h è coerente con d
			- alcuni di membri di S sono più specifici di h
		- Rimuovi da G ogni ipotesi  che è meno generale di un altra ipotesi in G
![[Pasted image 20240311204028.png]]

##### Valutazioni:
- il concetto target è appreso quando S e G convergono ad una singola identica ipotesi
- l'algoritmo converge verso il concetto target purchè:
	- non ci siano errori negli esempi di training
	- ci sia qualche ipotesi in H che descriva correttamente il concetto target
	- ci siano abbastanza esempi

#### Proprietà dell'Inferenza Induttiva:
- un algoritmo di apprendimento che non fa nessuna assunzione a priori riguardo l'identità del concetto target non ha nessuna base razionale per classificare le istanze non viste
- assunzione a priori = inductive bias
- inductive bias Candidate-Elimination: il concetto target appartiene allo spazio delle ipotesi
	- se questa assunzione è corretta e gli esempi sono privi di errori la classificazione di nuove istanze sarà corretta

#### Inductive Bias:
>Il bias induttivo di un algoritmo di learning è ogni minimo insieme di asserzioni B tali che per ciascun concetto target c e corrispondenti esempi di training D
>![[Pasted image 20240311205049.png]]
- è un modo per caratterizzare la politica di apprendimento per generalizzare oltre i dati osservati
- consente di confrontare diversi algoritmi di apprendimento sulla base del bias induttivo che adottano
	- Rote-learner: memorizza gli esempi. classifica x solo se è uguale ad un esempio memorizzato -> nessun bias induttivo
	- Candidate-Elimination: c ∈ H
	- Find-S: c ∈ H + tutte le istanze non negative a meno che non si provi l'opposto

##### Inductive Bias di Candidate-Elimination:
- l'algoritmo CE genera prima lo spazio delle versioni e poi classifica una nuova istanza *xi* mediante voto delle ipotesi nello spazio delle versioni
- l'inductive bias di questo algoritmo è l'assunzione che c ∈ H



