*gli appunti fanno riferimento alle lezioni del classroom 22/23 del corso di data mining & analytics*

| Lezione Precedente | [[01 - Introduzione al Machine Learning]] |
| ------------------ | ----------------------------------------- |
| Lezione Successiva | [[03 - Spazio delle Versioni]]            |


## Metriche di performance:
sono indicatori utilizzati per valutare l'efficacia dei modelli di Machine Learning, ne distinguiamo due:

##### Hard Prediction (Classification):
- predice una singola categoria per ogni istanza
	- alcuni strumenti che utillizza sono: *Accuracy, Error, Precision, Recall, Specificity, F1 score*
##### Ranking Prediction:
- restituisce un punteggio sottoforma di vettore per k classi $$s(x)=(s1(x),...,sk(x))$$
	- *si(x)* rappresenta il punteggio associato alla classe *Ci* per l'istanza *x*, il punteggio denota come la label si applica alla classe *Ci*
	- alcuni strumenti sono: *ROC, Precision-Recall Curves*
*alcuni esempi di metriche*
##### Probability Estimantion: 
- impara un modello che associa funzioni di probabilità alle singole classi
##### Regression: 
- impara ad approssimare che descrive meglio possibile i valori veri *f*

> La metrica scelta per valutare il modello è molto importante, questo influenza come le performance sono misurate e comparate

### Metrics for Hard Prediction
#### Confusion Matrix:
*caratteristiche*
- serve per problemi di classificazione, che prevede un output di due o più classi
- ciascuna colonna rappresenta la classe vera (actual)
- ciascuna riga rappresenta la classe predetta (predicted)
![[Pasted image 20240309094719.png]]
*obbiettivo*
- trovare una soluzione che massimizzi TP e TN
- nella migliore soluzione possibile FN e FP sono 0
- possiamo immaginare la matrice ideale come nel caso di una matrice che ha solo elementi nella diagonale
#### Tools: 
##### Accuracy:
 - è la proporzione degli esempi classificati correttamente, sul totale degli esempi
![[Pasted image 20240309095140.png]]
*considerazioni*
- possiamo anche considerare i numeri positivi P come TP+FN
- e i numeri negativi N come FP+TN
- va massimizzata
- non va bene in caso di dataset sbilanciati

##### Error Rate:
- è la proporzione di esempi classificati incorrettamente, sul totale degli esempi
![[Pasted image 20240309095549.png]]
*considerazioni*
- corrisponde al valore di 1 - Accuracy
- va minimizzato
- non va bene in caso di dataset sbilanciati

##### True Positive Rate and False Positivie Rate:
- è la proporzione di esempi classificati come positivi tra quelli che sono veramente positivi (True positive rate)
- è la proporzione di esempi classificati come positivi tra quelli che sono in realtà negativi (False positive rate)
![[Pasted image 20240309100303.png]]

##### Precision:
- la proporzione di esempi predetti come positivi che sono effettivamente positivi
![[Pasted image 20240309100439.png]]
*considerazioni*
- good quando vogliamo minimizzare i falsi positivi
- se la classe di minoranza è la classe di interesse e piccola, accuracy e performance delle classi di maggioranza non sono le quantità da ottimizzare, si utilizza precision invece

##### Recall (R) or Sensitivity:
- rappresenta il rapporto tra gli esempi positivi trovati e quelli che sono effettivamente tutti i positivi del sistema
![[Pasted image 20240309101153.png]]
*considerazioni*
- è uguale al TP rate
- good quando dobbiamo minimizzare i falsi negativi
- utile più che altro quando vogliamo riconoscere tutti i casi 

> Spesso Precision e Recall potrebbero essere in conflitto perchè:
> - nel caso stessimo analizzando il Recall e quindi volessimo minimizzare i falsi negativi, questo ci porterebbe ad avere un pool di positivi maggiore e di conseguenza una minore precisione
> - nel casi invece ci interessi la Precision e quindi volessimo minimizzare i falsi positivi, questo ci porterebbe ad avere un pool di negativi maggiore e quindi una probabilità di errore maggiore 

##### Specificity:
- rappresenta la precisione ma per i casi negativi
- è la misura del rapporto tra i veri casi negativi e tutti i casi negativi reali
![[Pasted image 20240309164340.png]]
- good quando dobbiamo minimizzare i falsi positivi 
- è l'opposto del recall
- dato da 1 - FP rate

##### F-measure or F1-score:
- considerando di volere un giusto compromesso tra precision e recall vogliamo una misura che li rappresenti entrambi
![[Pasted image 20240309165050.png|400]]
- se una delle due misure è 0 allora lo sarà anche la F-measure
- se una delle due misure è 1 allora lo sarà anche la F-measure
- maggiore è la differenza e più la F-measure tende a seguire la misura più bassa
- più le misure sono vicine più la F-measure sarà circa la metà dei due valori


##### Riassumiamo alcune metriche:
- **True Positive Rate (TPR):** è la misura di quanti elementi positivi ho trovato sul totale dei positivi reali
- **False Positive Rate (FPR):** è la misura di quanti elementi positivi ho trovato che sono in realtà negativi
- **Precision:** è la misura di quanti elementi positivi ho trovato che sono effettivamente positivi 
- **Recall:** (same as TPR)

### Metrics for Ranking Prediction:
#### Receiver Operating Characteristics (ROC) curve:
*considerazioni*
- rappresenta un grafico bidimensionale con FPR sull'asse delle x e TPR sull'asse delle y che illustra la performance di un classificatore in termini di benefits (TP) e costi (FP) 
- i punti all'interno del grafico sono rappresentati dalle coppie (fp rate e tp rate) ad ogni punto è associata una label
- più i punti sono collocati nell'angolo in alto a sinistra migliore è la performance (alto tp rate, basso fp rate)
- se il classificatore è situato nella metà del triangolo inferiore le prestazioni sono pessime (peggiori di previsioni random)
![[Pasted image 20240309193401.png]]

##### Toy Example:
*alcuni classificatori restituiscono un punteggio generico o una probabilità* è possibile integrare a questi classificatori un valore di soglia per produrre una variabile discreta (binaria), ad esempio il classificatore produce *yes* se il valore è sopra alla soglia oppure *no* se è inferiore
![[Pasted image 20240309194502.png]]
- in questo esempio abbiamo 20 istanze con 10 positive e 10 negative
- conosciamo a priori i valori reali delle classi
- abbiamo un punteggio di score che utilizziamo arbitrariamente per determinarne il valore di soglia
- calcoliamo i relativi fpr e tpr ad ogni istanza per creare la curva
![[Pasted image 20240309200659.png]]

##### Real Example:
![[Pasted image 20240309200831.png]]
- una reale applicazione prevederebbe dataset molto più grandi quindi la curva sarebbe più stondata e i valori di soglia, tpr e fpr molto più precisi

##### Area under the ROC curve (AUC ROC):
*per comparare i classificatori occorre rappresentare l'indice di performance di ROC con un valore*
- l'area sotto la curva ci fornice questo valore
	- maggiore è l'area migliori sono le performance
	- varia tra 0 e 1, considerando che un valore < 0.5 indicherebbe delle performance pessime
###### Caso Ideale:
AUC = 1
- significa che il sistema è in grado di distinguere perfettamente tra positivi e negativi
 ![[Pasted image 20240309202401.png]]![[Pasted image 20240309202425.png|300]]

###### Caso Medio:
0.5 < AUC < 1
- il sistema non distingue perfettamente tutti i positivi dai negativi ma ha comunque una buona precisione
![[Pasted image 20240309202658.png]]![[Pasted image 20240309202711.png]]

###### Caso Peggiore: 
AUC = 0.5
- è il caso peggiore perchè il sistema non è in grado di distinguere positivi a negativi
![[Pasted image 20240309202944.png]]![[Pasted image 20240309202954.png]]

###### Caso Particolare: 
AUC = 0
- se l'area fosse 0 il sistema distingue benissimo le classi ma inverte positivi con negativi
![[Pasted image 20240309203103.png]]![[Pasted image 20240309203114.png]]

#### Precision Recall (PR) curve:
- alternativa alla curva ROC per casi in cui il dataset sia particolarmente sbilanciato
- rappresenta sempre un grafico bidimensionale ma con i valori di Recall (tpr) nell'asse delle x e i valori di Precision nell'asse delle y
- le performance migliori si trovano nell'angolo in alto a destra del grafico 
![[Pasted image 20240309203849.png]]
- mentre la curva ROC misura le performance di un modello nel distinguere positivi da negativi
- la curva PR misura la precisione del modello nel classificare correttamente i veri positivi
*es.* 
- immaginiamo di avere un dataset con molti più negativi che positivi, potremmo essere più interessati al valore di accuratezza del modello nel classificare i positivi correttamente.
- PR non utilizza true negative all'interno delle formule quindi i risultati non saranno "truccati" dal grande valore dei negativi
![[Pasted image 20240309210444.png]]![[Pasted image 20240309210500.png]]
- nelle immagini possiamo vedere come si comportano le curve con dataset bilanciati o sbilanciati
- ROC contiene misura di true negative, mentre PR no

##### Multi-Class Classifiers
- è possibile utilizzare i metodi ROC e PR anche su dataset che prevedono più di due classi
- tuttavia spesso è molto oneroso e non conveniente 
- alcuni metodi possono essere quelli di raggruppare le classi a turno in modo da averne sempre due e di applicare le misure di performance
### Metrics for Regression:
*altre tecniche per le valutazioni delle performance riguardano la regressione*
#### Mean Squared Error (MSE):
*questa misura è la rappresentazione della varianza dei residui della regressione*
- mostra quanto distanti sono i valori predetti dai valori effettivi
- si calcola come la differenza al quadrato dei valori reali e dei valori predetti
- è più sensibile di MAE agli errori grandi
- è sempre non negativa
- più vicino è allo zero migliori sono le prestazioni del modello
![[Pasted image 20240310112926.png]]
- *yi* sono i valori effettivi, *yi segnato* sono i valori predetti, N rappresenta il totale dei punti

#### Root Mean Squared Error (RMSE):
*è la misura della deviazione standard dei residui della regressione*
- mostra quanto i dati sono distanti dalla media 
- è sempre non negativa
- più vicino è allo zero migliori sono le prestazioni del modello
![[Pasted image 20240310113526.png]]
- è uguale alla MSE ma sotto radice
#### Mean Absolute Error:
- mostra la differenza tra valori effettivi e valori predetti in valore assoluto
- usata quando vogliamo penalizzare di meno gli errori (è meno sensibile agli errori grandi)
- è sempre non negativa
- più vicino è allo zero migliori sono le prestazioni del modello
![[Pasted image 20240310114151.png]]

#### Riassumendo le Metrics for Regression:
- **MSE:** errori più grandi pesano di più di quelli minori
- **RMSE:** mantiene la proprietà di MSE di penalizzare i grandi errori ma utilizzando la radice riporta i valori all'unità di misura originali, è la scelta migliore per comparare l'accuratezza di modelli di regressione
- **MAE:** è meno sensibile ad errori grandi per via del valore assoluto

### Metrics for Probability Estimantion:
*la probabilità determina quanto è possibile che un evento si verifichi o meno, fornendo una misura dell'incertezza*
*è possibile fare inferenza in vari modi noi ci concentriamo sulla probabilità*
#### Probability Theory:
- **A:** rappresenta l'evento
- **P(A):** rappresenta la probabilità che A si verifichi
- la funzione di probabilità restituisce un numero 0 < P() < 1
- se P() = 1 l'evento è certo
- P(A v B) = P(A) + P(B) disgiunzione se A e B sono mutualmente esclusivi
- **Frequentist Approach:** la probabilità è relativo alla frequenza, utilizza la ripetizione degli esperimento per la misura
- **Bayesian Approach:** nel caso in cui non sia possibile effettuare degli esperimenti la probabilità diventa una stima soggettiva, *es.* probabilità che si verifichi un furto

#### Metriche:
##### Probabilità Congiunta:
- rappresenta la probabilità che due eventi siano verificati allo stesso tempo
$$P(H ∧ F) = P(H, F)$$
*nell'esempio H rappresenta avere il mal di testa, F avere l'influenza*
![[Pasted image 20240310121526.png]]

##### Marginalization / Sum Rule:
- la probabilità di A può essere scritta anche come:
$$P(A) = P(A, B) + P(A, -B)$$
- questo perchè la probabilità di P(B) + P(-B) = 1

##### Conditional Probabilities:
- rappresenta la probabilità che si verifichi A dato B
![[Pasted image 20240310122259.png]]
- se P(B) = 0 allora P(A|B) non è definita
*es.*
![[Pasted image 20240310122348.png]]
- nell'esempio la probabilità di avere il mal di testa dato che ho l'influenza è del 50%

##### Product Rule:
- possiamo ricavare dalla formula della probabilità condizionata la probabilità congiunta
![[Pasted image 20240310122603.png]]
- la formula può essere nuovamente invertita per ricavarne la probabilità condizionata 

##### Bayes Theorem:
- secondo questo teorema possiamo trovare una correlazione tra P(A|B) e P(B|A)
![[Pasted image 20240310122805.png]]
*es.*
![[Pasted image 20240310122837.png]]
- notiamo che la probabilità di avere l'influenza dato il mal di testa è del 12%

##### Chain Rule:
- è possibile trovare la probabilità congiunta di N eventi tramite questa proprietà
![[Pasted image 20240310123126.png]]

#### Multivalued Hypothesis
- fino ad ora abbiamo visto i casi in cui le proposizioni prevedano due valori (binary), ad esempio la probabilità di avere o non avere l'influenza o il mal di testa
- è possibile però in alcuni casi avere più preposizioni ed esempio nel caso di un semaforo S i valori sarebbero verde, giallo, rosso

##### Discrete Random Variables:
- una variabile random discreta *V* può assumere *vi* valori
- *V = vi* è la proposizione
- *P(vi)* corrisponde a *P(V=vi)*
- la distribuzione di probabilità si indica con P(V)
- la somma di tutte le probabilità di *vi* è uguale a 1

*anche nel caso di multivalued hypothesis valgono le stesse proprietà di probabilità*

##### Marginalization / Sum Rule:
- in questo caso si determina la probabilità di x data la probabilità congiunta di x e y con y che può assumere più valori
![[Pasted image 20240310124300.png]]

##### Conditional Probabilities:
- P(a|b) credenza che A=a dato che conosco B=b
![[Pasted image 20240310124710.png]]
*nell'immagine vediamo sia la product rule, prima formula, sia la probabilità condizionata, seconda formula*
![[Pasted image 20240310124806.png]]
*nella seconda immagine invece è rappresentata la formula del teorema di Bayes*

##### Continuous Random Variables:
- nel caso in cui si abbia a che fare con una variabile continua V questa può assumere valori all'interno di un intervallo [a, b]
- V è descritta da una **probability density function** p:[a,b]->R>=0
- p(v) è rappresentata così
![[Pasted image 20240310125209.png]]

*valgono anche qui le stesse proprietà*

##### Marginalization / Sum Rule:
![[Pasted image 20240310125334.png]]

##### Probabilità Condizionata e Product Rule:
![[Pasted image 20240310125423.png]]

##### Mixed Distribution:
- potremmo avere anche distribuzioni miste con variabili continue e discrete
**Congiunzione:** 
- se una delle due variabili è continua la congiunzione è una funzione di densità
	- X discrete, Y continuous: p(x,y)
**Condizionata:**
- X discrete, Y continuous: P(x|y)
- X discrete, Y continuous, Z discrete: p(x,y|z)


