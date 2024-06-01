*gli appunti fanno riferimento alle slide della lezione 8 del classroom di data mining & analytics del 22/23*

| Lezione Precedente | [[06 -kNN]]            |
| ------------------ | ---------------------- |
| Lezione successiva | [[08 - Linear Models]] |

## Bio-Inspired Learning:
- ispirandoci al cervello umano possiamo immaginare un tipo di learning basato su neuroni

*alcune notazioni*
- *neurone* è un unità che invia segnali elettrici
- il *rate of firing* ci dice come è *attivato* un neurone
- un singolo *neurone* può essere collegato ad altri
- altri neuroni *sparano* diversamente
- il *rate of fire* è definito da quanto è forte la connessione
![[Pasted image 20240319220751.png]]
*nell'immagine è rappresentata la struttura di un neurone dove i dendrites rappresentano l'input e l'axom l'output*

>il caso biologico è molto più complesso di così, possiamo comunque pensare ad un modello ispirato nel modo di funzionare, per il momento pensiamo:
>- ad un modello con un singolo neurone
>- che riceve input da D-many altri neuroni per ogni feature
>- la forza degli input è data dal loro valore
>- ogni input ha associato un peso che vengono sommati a tutti gli input
>- basandosi su questo si decide se sparare o no
>- firing è interpretato come un esempio positivo e viceversa
>- se la somma è positiva si spara

![[Pasted image 20240319221501.png|600]]
*rappresentazione del funzionamento dell'attivazione di un neurone*

### Funzione di Attivazione:
- input vector: $$x = <x1, x2, ..., xd> ∈ R$$
- pesi: $$ <w1, w2, ..., wd>$$
- funzione di attivazione:
	![[Pasted image 20240319222014.png|200]]
	- se l'attivazione è positiva (es. a > 0) è predetto un esempio positivo altrimenti negativo
	- il segno di *a* è definito da
		![[Pasted image 20240319222158.png]]
	- dove *+1* indica una classe positiva e *-1* negativa	
		- features con peso = 0 sono ignorate
		- peso > 0 activation increase
		- peso < 0 activation decrese

#### Bias:
- in molti casi vogliamo avere una soglia di attivazione diversa da 0 ma piuttosto potremmo considerare un valore theta
- introduciamo per questo l'utilizzo di un *bias*
- il bias è una costante che viene aggiunta alla somma per aiutare il modello ad avere una precisione migliore nella classificazione
	![[Pasted image 20240319222810.png]]

### Perceptron:
>E' un algoritmo di learning per modelli neurali che ha le seguenti caratteristiche
>- *online* ovvero non considera tutti i dati allo stesso tempo ma uno alla volta
>- *error driven* fintanto che l'algoritmo si comporta bene non aggiorna i suoi parametri 

*funzionamento*
- l'algoritmo mantiene una supposizione sui buoni parametri (pesi e bias) durante l'esecuzione
- processa un esempio alla volta e fa predizione
- controlla se la predizione è corretta
	- se è corretta non fa nulla
	- se non lo è cambia i suoi parametri in modo da comportarsi meglio la prossima volta sullo stesso esempio
- poi passa all'esempio successivo
- quando termina gli esempi del training set, ripete il giro per uno specifico numero di iterazioni

![[Pasted image 20240319224001.png]]
*pseudocodice sul funzionamento del training dell'algoritmo*

![[Pasted image 20240319224045.png]]
*pseuocodice sul funzionamento del test dell'algoritmo*

*errori*
- immaginiamo di compiere un attivazione *a* e di commettere un errore, (es. a < 0)
- aggiorniamo ora *pesi* e *bias*, chiamiamoli $$w1', ..., wd', b'$$
- supponendo si osservare lo stesso esempio di nuovo e di voler calcolare una nuova attivazione *a'*
	![[Pasted image 20240319224838.png|400]]
- quindi notiamo che ora la nuova attivazione *a'* è corretta per l'esempio positivo

*considerazioni*
- l'unico iperparametro nell'algoritmo percettrone è *MaxIter* ovvero il numero di volte su cui iterare il training set
- se il numero di iterazioni è troppo alto è probabile avere **overfitting**
- se il numero delle iterazioni è troppo basso è probabile avere **underfitting**
![[Pasted image 20240319225300.png|400]]
*l'immagine mostra gli errori compiuti in train e test all'aumentare delle iterazioni*
- **soluzione ad overfitting:** early stopping
	- controllare le performance dopo ogni iterazione e smettere di ottimizzare l'algoritmo quando le performance si appiattiscono

- un altro caso a cui prestare attenzione è **l'ordinamento dei dati**
- se i dati fossero ordinati (es. tutti i positivi e tutti i negativi) per come è strutturato l'algoritmo dopo una serie di esempi comincerà a prevedere automaticamente la classe corretta senza modificare i suoi parametri essendo raggruppate
	- supponiamo di avere raggruppati 500 esempi positivi e 500 esempi negativi
	- dopo i primi 5 esempi positivi l'algoritmo deciderà che tutti i successivi sono positivi
	- poi magari nel gruppo dei negativi dopo i primi 10 l'algoritmo deciderà che sono tutti negativi
	- **le predizioni potrebbero anche essere corrette ma l'algoritmo avrebbe imparato solo su 15 esempi utili**
- la soluzione in questo caso è quella di **permutare** i dati ciclicamente ad esempio ad ogni iterazione
![[Pasted image 20240319230315.png|400]]
*l'immagine mostra le performance di test con e senza permutazione dei dati*

##### Geometric Interpretation:
- il confine decisionale *B* definisce precisamente dove il segno di attivazione *a* cambia da *-1* a *+1*
- è rappresentato dal set di punti *x* che raggiungono attivazione 0

*consideriamo prima il caso in cui non sia presente un bias o sia uguale a 0*
	![[Pasted image 20240319231501.png]]
- il prodotto scalare tra *w* ed *x* è 0 se e solo se sono perpendicolari
- il confine decisionale è semplicemente il piano perpendicolare a *w*
	![[Pasted image 20240319232030.png]]
- non ci importa sapere quanto distante è il punto dal confine ma solo da che parte sta
- è pratica comune normalizzare i vettori dei pesi *w* per avere lunghezza 1 (es. ||w||=1)
	![[Pasted image 20240319233921.png]]
- il valore del prodotto scalare *w * x* rappresenta la distanza di *x* dall'origine proiettato nel vettore *w* 
- la proiezione lungo *w* di ogni punto rappresenta esattamente l'attivazione di quell'esempio *senza bias*

*consideriamo ora il caso nel quale il bias sia presente*
- il ruolo del *bias* è quello di spostare il confine decisionale distante dall'origine nella direzione di *w*
- se il *bias* è positivo il boundary è spostato distante da *w* se il *bias* è negativo è spostato attraverso *w*
![[Pasted image 20240320164017.png|400]]
*l'immagine mostra l'update di un percettrone mostrato geometricamente*

### Perceptron Convergence and Linear Separability

- la *convergenza* dell'algoritmo perceptron è data quando compie un intera iterazione senza fare alcun update ovvero ha classificato correttamente tutti gli esempi 
- geometricamente significa che è stato trovato un iperpiano che divide correttamente gli esempi dai positivi ai negativi, non sempre tuttavia è possibile

>Possiamo generalizzare dicendo che nell'algoritmo perceptron se i dati sono linearmente separabili allora convergerà altrimenti se i dati non sono linearmente separabili non convergerà mai

 >introduciamo la notazione di *margine*, ovvero se i dati sono separabili da un iperpiano, la distanza che c'è tra questo e il punto piu vicino è chiamata margine, maggiore è la distanza più semplice risulterà la separazione
 
 - dato un dataset *D*, un vettore di pesi *w* e un bias *b*, il margine di *w*, *b* su *D* è dato da:
	 ![[Pasted image 20240320165653.png]]
- il margine è ovviamente definito solo se vi è un iperpiano che separa i dati, in questo caso troviamo il punto della minima attivazione (che come abbiamo detto prima rappresenta il punto più vicino all'iperpiano) definendone il margine

- i margini sono denotati con la lettera greca *γ* gamma
	![[Pasted image 20240320170025.png]]


> [!NOTE] Perceptron Convergence Theorem
>supponiamo che l'algoritmo perceptron sia eseguito su un dataset *D* linearmente separato, con margine  *γ > 0*.
>Assumiamo che *||x||<=1* per ogni x ∈ D. Allora l'algoritmo congergerà dopo al massimo *1/γ^2* aggiornamenti di pesi


### Voting and Averaging:
- immaginiamo di avere una situazione con 10000 esempi nel dataset in cui il nostro algoritmo dopo i primi 100 si comporta bene senza aggiornare i pesi e all'ultima iterazione faccia un errore e ne modifichi i pesi
- ovviamente questa situazione non è ideale perchè un esempio rovina i pesi che si sono comportati bene nel 99,99% degli esempi
- introduciamo perciò la notazione di *voting e averaging*

#### Voting:
- nel processo di apprendimento del percettrone viene ricordato ogni iperpiano quanto "sopravvive"
- nella fase di test ogni iperpiano incontrato durante il training vota nella classe di un esempio di test
- se un iperpiano è sopravvissuto per 20 esempi riceverà un voto di 20, se è sopravvissuto solo per un esempio allora otterrà un voto
![[Pasted image 20240320171608.png]]
*c è la variabile che conteggia quanti round sopravvive un iperpiano, quindi con pesi e bias uguali*
- purtroppo in realtà è un sistema impraticabile per quantità di aggiornamenti elevate, intatti significherebbe ad esempio che se ci sono 1000 aggiornamenti bisogna salvare 1000 vettori di pesi

#### Averageing:
- una tecnica migliore è quella dell'averaged perceptron
- l'idea è simile, mantenere un insieme di vettori di pesi e  tempi di soppravvivenza
- tuttavia in fase di test, la predizione è fatta sulla base del vettore di pesi medio invece che sul voting
![[Pasted image 20240320172155.png]]
![[Pasted image 20240320172242.png]]
*in blu l'averaged weight vector e in rosso l'averaged bias*

### Limitations:
- come già annunciato purtroppo questo algoritmo funziona solo in cui i dati siano separabili e il decision boundary può essere solo lineare