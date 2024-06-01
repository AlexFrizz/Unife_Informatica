*gli appunti fanno riferimento alla lezione 1 del corso di data mining & analytics*

| Lezione Successiva | [[02 - Metriche di Performance e Probabilità]] |
| ------------------ | ---------------------------------------------- |

## Machine Learning:

> [!NOTE] Definizione Machine Learning
> E' la materia che si occupa di creare sistemi che apprendono o migliorano le performance dato un dataset dal quale fare analisi e previsioni
> 
>permette ai computer di imparare e fare inferenza dai dati

*è un campo multidisciplinare che prende concetti da materie come AI, probabilità e statistica, matematica, filosofia, psicologia, neurobiologia ecc.*
- siamo sommersi dai dati, l'obbiettivo è quello di trarre informazioni utili da questi

*i campi sono di utilizzo sono principalmente due*
- **Knowledge Extraction:** per essere utilizzati in knowledge-based system (es. classification system) oppure per essere presentati alle persone (es. scopi scientifici)
- **Improvement Performance:** per migliorare un sistema (es. capacità di un robot)

### Applications:
- **Prediction:** determina cosa possiamo predirre da un fenomeno
- **Description:** determina come possiamo descrivere o capire un fenomeno

>qualsiasi sia l'applicazione gli algoritmi di ML estraggono strutture dai dati chiamate (**models**)

>la qualità e la dimensione dei dati sono elementi cruciali per il successo della predizione/descrizione del fenomeno
*alcune delle misure di qualità sono time complexity, space complexity, sample complexity*

![[Pasted image 20240310184908.png]]

### Notazioni:
- **Dataset:** l'intera collezione di dati (*data points*)
- **Example:** un singolo data point *x* del dataset
	- sinonimi: *osservazione, istanza, elemento, riga, record*
- **Feature:** insieme di attributi, spesso rappresentati come vettore di *x* associato all'esempio
	- sinonimi: *predittori, input, variabili indipendenti, attibuti*
- **Label:** categoria assegnata agli esempi (**non sempre disponibile**)
	- sinonimi: *classe, domanda, **y**, risposta, output, variabile dipendente, **target***

### Paradigmi (tipi di learning):
- **Supervised:** i data points sono etichettati (ioutput conosciuti)
- **Unsupervised:** i data points non sono etichettati (non si conosce l'output)
- **Semi-Supervised:** i data points sono parzialmente etichettati (si conosce l'output solo di alcuni dati)
- **Reinforcement:** i data points hanno etichette implicite 
![[Pasted image 20240310190428.png]]
*nell'immagine le righe rappresentano come i dati sono etichettati, mentre le colonne per cosa sono utilizzati i modelli imparati*

*common settings*:
- supervised learning per predictive model
- unsupervised learning per descriptive model

#### Supervised Learning:
>conosciamo le coppie (input, correct output) nella fase di **training**
>*per ogni esempio i c'è una feature con la corrispondente y: (xi, yi)*
- vogliamo predirre dato il valore della feature di un nuovo elemento *x* il valore sconosciuto del suo target *y*
![[Pasted image 20240310191344.png]]
*nella figura vediamo che abbiamo una serie di valori per x1 e x2 ai quali corrisponde un valore conosciuto, il modello dopo il training ci fornice il valore target di due nuovi esempi x1 e x2*

#### Unsupervised Learning:
> invece di avere le coppie (input, correct output) abbiamo (input, ?) nei dati di training
>  *estrae informazioni e cataloga questi dati, usiamo tecniche come clustering o deinsity estimation*
- vogliamo suddividere gli elementi in sottogruppi correlati tra di loro (feature selection e feature extraction)
![[Pasted image 20240310192049.png]]
*nell'immagine vediamo una serie di dati che che possiamo dividere in quattro sottogruppi per come sono posizionati nel grafico*

#### Reinforcement Learning:
>invece delle coppie (input, correct output) abbiamo (input, some, output, grade for this output)
>*il modello dopo una serie di operazioni e di feedback impara quale è il modo migliore di svolgere un compito
- ad ogni azione riceve un reward (feedback) che può essere negativo o positivo, in base a questo il sistema impara se sta facendo bene o male l'obbiettivo è quello di ottenere il reward comulativo più grande possibile
![[Pasted image 20240310192532.png]]
*vediamo nell'immagine una serie di step che ne rappresenta il funzionamento, chi compie le azioni viene chiamato agente intelligente e le azioni avvengono all'interno di un ambiente*

#### Semi-Supervised Learning (SSL):
> abbiamo una parte di coppie (some input, correct output) e una parte di coppie (some input, ?)
> il sistema utilizza i dati etichettati per trovarne un pattern e prova a riprodurli con i dati non etichettati 

![[Pasted image 20240310193030.png]]
*nell'immagine i punti colorati (non blu) rappresentano i dati etichettati*

### Some Definition:
*alcune notazioni importanti che riguardano tutti i tipi di learning*
- **Loss Function:** una funzione che misura la differenza tra i valori predetti e i veri valori etichettati (quando disponibili) 
- **Hypothesis Set:** un insieme di funzioni (feature vectors) che l'algoritmo usa per mappare gli input agli output
- **Training Set:** gruppo di esempi utilizzati per addestrare un algoritmo
- **Validation Set:** gruppo di esempi utilizzati per affinare gli iperparametri di un algoritmo utilizzando dati etichettati
- **Test Set:** gruppo di esempi utilizzati per valutare le performance di un algoritmo utilizzando dati etichettati
*tutti e tre i tipi di set sono separati e tipicamente sono suddivisi in 70% del dataset adibito al training, 20% al validation e 10% per test*
![[Pasted image 20240310194513.png]]
*l'immagine mostra il funzionamento e l'utilizzo dei diversi set per il learning di un algoritmo*

### Cross Validation:
*non sempre è possibile avere grandi quantità di dati per l'addestramento, per questo potremmo avere ad esempio un test set troppo piccolo*
- Solution: ripetere training e test mescolando i dati suddividendoli in subset scelti a caso
#### K-fold cross-validation
*es. 10-fold cross-validation*
- divido il set in 10 parti di dimensione uguale, ne uso 9 insieme per training e 1 per test
- ripeto questa operazione 10 volte cambiando la parte di test ogni volta
- alla fine calcolare la media di performance di ogni ripetizione
*stiamo valutando lo stesso algoritmo k volte, non valutando k algoritmi*

#### Leave-one-out cross-validation:
- setto k = n e alleno l'algoritmo su tutto il set tranne una istanza di test ripeto n volte

### (Supervised) Learning Stages:
- (1) si comincia dalla collezione di esempi (dataset)
- (2) si partiziona in maniera casuale il dataset in training, validation e test set
- (3) l'utente sceglie le feature più rilevanti per l'addestramento
- (4) si sceglie l'algoritmo di apprendimento migliore
- (5) fase di training, si usano le feature selezionate per il training
- (6) fase di validazione, per ogni parametro l'algoritmo sceglie l'hypothesis dall'hypothesis set selezioniamo tra queste quelle che risultano nelle migliori performance
	- *questo è l'output model*
- (7) fase di test, usando quelle hypothesis prediciamo le label del test set
- (8) valutiamo le performance dell'algoritmo utilizzando strumenti come loss function
*5, 6, 7, 8 possono essere sostituite da cross-validation*

*precisazioni*
- cominciamo con un insieme di candidate hypothesis che dovrebbero rappresentare f
- scegliamo un hypothesis *g* dall'hypothesis set tramite un algoritmo di apprendimento
- usiamo *g* per nuove predizioni -> goal g = f
	- *g(xn) = yn*
	- *g* è l'output model

![[Pasted image 20240311093725.png]]
*l'immagine mostra una suddivisione tra tipi di learning e algoritmi*

*in sintesi*
- **Regressione:** quando si ha a che fare con variabili continue (valori all'interno di un intervallo e necessariamente numerici)
- **Classificazione:** quando si ha a che fare con variabili discrete (assumono un valore preciso e possono essere anche non numeriche)

#### Regression:
*esistono vari tipi di regressione:*
- **Linear Regression:** modello lineare dove *g* è la combinazione lineare di input e features con pesi applicati ad ogni feature
![[Pasted image 20240311094923.png]]
- **Polinomial Regression:**
![[Pasted image 20240311095029.png]]
- **Logistic Regression:**
![[Pasted image 20240311095051.png]]

- Output e Input sono valori numerici
- l'obbiettivo è trovare i pesi giusti per ottenere il modello con l prestazioni migliori
- Linear e Polinomial sono facili da visualizzare e in due dimensioni
- Logistic da come output un valore di probabilità compreso tra 0 e 1, il valore può essere interpretato come la probabilità che l'evento si verifichi

##### Fitting Training and Test Data:
- **Fitting (Adattamento):** fase in cui il modello si addestra sui dati di training, implica l'ottimizzazione dei parametri in modo da ottenere le performance migliori
![[Pasted image 20240311100659.png]]
*nella figura si mostra la fase di fitting del modello in cui si trova la combinazione migliore per rappresentare i dati* 

- **Test Data:** una volta predetti i valori di output di nuove variabili nella fase di test si effettuano delle misure di performance per verificarne il comportamento
![[Pasted image 20240311100532.png]]
*nella figura si vedono i valori predetti (quadrati blu) e la misura dell'errore dai valori effettivi (punti rossi)*


#### Generalization, Underfitting and Overfitting:
L'obbiettivo in ML è quello di avere buone performance del modello sia che si conoscano o meno i dati
- **Generalization:** è la proprietà di performare bene in dati nuovi non visti

*due chiavi fondamentali in tutto questo sono overfitting e underfitting*
- **Underfitting:** succede quando il modello non è in grado di ottenere un errore sufficientemente basso in training set
- **Overfitting:** accade quando la differenza tra training error e test error è troppo grande (performance ottime in training e pessime in test)

*possiamo controllare underfitting e overfitting di un modello alterando la sua **capatity** (l'abilità di fittare una grande varietà di funzioni target)*
- modelli con **bassa capacity:** difficoltà nel fitting del training set
- modelli con **alta capacity:** overfit perchè sono troppo abituati ai dati di training
>un modo di controllare la capacità di un algoritmo è scegliere lo **spazio delle hypothesis** 
>
>la capacità di un modello va regolata in base a quella che è la complessità del task da compiere e la quantità di dati di training

![[Pasted image 20240311102526.png]]
*l'immagine mostra alcune rappresentazioni di underfitting e overfitting su un algoritmo di regressione*

![[Pasted image 20240311102654.png]]
*rappresentazione concettuale di underfitting e overfitting, all'aumentare della capacità il gap aumenta*

#### Bias and Variance:
##### Bias:
*è un tipo di errore sistematico che si verifica quando il modello favorisce costantemente un risultato rispetto a un altro, indipendentemente dalla qualità dei dati di addestramento*
- rappresenta la differenza tra i valori predetti dall'algoritmo e i valori effettivi corretti
- Y' è **unbiased** se il bias è 0, significa che E(Y') = Y
- **se i valori medi predetti sono molto distanti dai valori effettivi il bias è alto**
- High Bias -> modello troppo semplice -> underfitting
![[Pasted image 20240311103800.png]]

##### Variance:
*è la stima che la funzione target cambi se usati differenti dati di training*
- variance(Y') = E[(Y' - E(Y'))^2]
- idealmente non dovrebbe cambiare di molto tra diversi training set
- **se i valori predetti sono molto più alti della loro media la varianza è alta**
- **Low Variance:** suggerisce piccoli cambiamenti nella stima della funzione target al cambio del training set
- **High Variance:** suggerisce grande cambiamenti nella stima della funzione target al cambio del training set
	- questo potrebbe significare random noise presente bei dati -> overfitting
![[Pasted image 20240311104422.png]]

![[Pasted image 20240311170647.png]]
*questa immagine mostra in sunto come si comportano i modelli al variare del bias e della varianza*

![[Pasted image 20240311170835.png]]
*è qui invece rappresentato un grafico del comportamento di un modello al variare di bias e varianza*

### Learning with Different Protocol
*una serie di tipi di learning che utilizzano protocolli diversi per l'addestramento*
- **Batch Learning:** i dati sono presentati all'algoritmo nella loro interezza all'inizio del processo di apprendimento (comune in supervised learning)
	- *es.* spam filter o cancer classifier
- **Online (Incremental) Learning:** il dataset è dato all'algoritmo un esempio alla volta
	- *es.* utile in casi di limitazioni hw o dati che devono essere processati al momento (sensor data)
	- il modello necessita di essere aggiornato ogni volta che arriva un nuovo data point
	- supervised learning se i dati sono labeled o reinforcement learning
- **Active Learning:** durante il training uno user interagisce con l'algoritmo a proposito della previsione *y* di un esempio *x*, concettualmente un supervised learning interattivo
	- *es.* natural language processing (NLP)
	- tipologia human-in-the-loop
	- è un tipo di semi-supervised learning

### Types of Approach:
*possiamo avere vari tipi di approcci per la creazione del modello*
#### Data-Driven Approach:
questo approccio si basa nel creare un sistema che possa identificare la risposta corretta basandosi sull'aver "visto" grandi quantità di esempi possibilmente etichettati, e addestrato su questi per ottenere la giusta prediction
![[Pasted image 20240311172854.png]]

#### Model-Driven Approach:
approccio che mira a estrarre conoscenze e derivare decisioni da regole e rappresentazioni esplicite
- Representation languages:
	- propositional
	- first order
		- *cat(x) <- baffi(x), quattro-zampe(x)*

#### Symbolic and Sub-Symbolic Approach:
- **Symbolic:** approccio basato sulla manipolazione di simboli per rappresentare concetti del mondo reale, spesso combinati utilizzano regole logiche per risolvere i problemi
- **Sub-Symbolic:** approccio basato sull'apprendimento di modelli direttamente dai dati senza rappresentazione esplicita della conoscenza, spesso utilizzati per imparare relazioni tra input e output
![[Pasted image 20240311174238.png]]
![[Pasted image 20240311174253.png]]

