*gli appunti fanno riferimento alle slide della lezione 8 e 9del classroom di data mining & analytics del 22/23*

| Lezione Precedente | [[07 - Perceptron]]     |
| ------------------ | ----------------------- |
| Lezione successiva | [[09 - Neural Network]] |
## Optimization Framework for Linear Models:
- l'obbiettivo dell'algoritmo perceptron è quello di trovare un iperpiano che separi i dati
- nel caso in cui i dati non siano linearmente separabili, magari si vuole pensare di trovare l'iperpiano che faccia il minor numero di errori possibili sui dati di training
	![[Pasted image 20240320174520.png|400]]
	*riassumiamo quello che abbiamo appena detto con una formula matematica*
- in questa espressione ottimizziamo sulle due variabili *w* e *b* 
- l'objective function (ottimizzazione) è semplicemente l'error rate (0/1 loss) del classificatore con parametri *w* e *b*
- 1[ ] è l'indicator function: vale 1 se [ ] è vero e 0 altrimenti

- la *loss function* ci dice come vengono predetti i valori in confronto agli originali, ovvero la misura dell'errore
- esempi di loss function:
	- **regressione:** (variabili continue)
		![[Pasted image 20240320175323.png]]
	- **classification:** (variabili discrete)
		![[Pasted image 20240320175355.png]]
- l'efficienza dipende dal margine dei dati per il percettrone

- è importante sottolineare che ci stiamo riferendo solo all'applicazione nei dati di training e non nel test, per la natura del problema (NP-hard) è mlto difficile minimizzare o annullare qualsiasi forma di errore in separazioni non lineari
- per provare a trovare una soluzione per generalizzare nei dati di test bisogna assicurarsi di non avere overfit sui dati
- introduciamo per fare questo un *regolatore* nei parametri del modello *R(w, b)*
![[Pasted image 20240320180732.png]]
- stiamo cercando di ottimizzare un trade-off tra una soluzione che sia basso training error (primo termine dell'eq) e una soluzione che sia semplice (secondo termine)
- possiamo pensare al regolatore come all'iperparametro che regola la pofondità di un decision tree
- in questa forumlazione *λ* diventa un iperparametro

### Convex Surrogate Loss Function:
- una delle ragioni per cui ottimizzare 0/1 loss sia così difficile, è perchè ogni piccola modifica di *w* e *b* potrebbe avere un grosso impatto sull'ottimiazzazione
- ad esempio se abbiamo un esempio positivo e *wx + b = -0.00000001*, aggiustando *b* di *0.00000011* avremmo ridotto l'error rate di *1*, allo stesso modo aggiustandolo di *0.00000009* non si avrebbe avuto alcuna modifica
- questo rende molto difficile capire quali sono le vie corrette per aggiustare i parametri
![[Pasted image 20240320182517.png]]
*questa immagine mostra come se si ha un margine positivo allora si ha una loss di 0 altrimenti una loss di 1*
![[Pasted image 20240320182650.png]]
*provando ad approssimare otteniamo questa forma*

- introduciamo ora le notazioni di funzione *concava* e *convessa*
- le funzioni convesse sono facili da minimizzare
![[Pasted image 20240320183118.png]]

- **convex surrogate loss function**
- siccome le funzioni convesse sono facili da ottimizzare vogliamo approssimare 0/1 loss con una funzione convessa (surrogate loss)
- la surrogate losses che costruiamo sarà sempre maggiorante della vera loss function questo garantisce che se riusciamo a minimizzare la surrogate loss stiamo minimizzando anche la vera loss
![[Pasted image 20240320183725.png|700]]
*l'immagine mostra quattro funzioni surrogate loss comuni: hinge, logistic, exponential, squared*

![[Pasted image 20240320183947.png]]
*l'immagine mostra le formule per le varie surrogate loss, y è il vero valore (+1, -1) e y_segnato il valore predetto dato da w * x + b*

*differenze*
- errori:
	- nel caso di hinge e logistic loss la crescita della funzione quando *yy_segnato* vanno negativi è lineare
	- nel caso si squared ed exponential loss è super-linear ovvero si preferisce sbagliare due piccoli esempi piccoli piuttosto che uno grande
- predizioni corrette:
	- quando *yy_segnato > 1* hinge loss non fornisce più informazioni
	- logistic ed exponential loss possono ancora essere migliorate
	- squared loss 

### Weigth Regularization:
![[Pasted image 20240320210704.png]]
- l'ottimizzazione *R* deve essere convessa
- vogliamo dei pesi *w* piccoli per semplificare l'ottimizzazione pensando che gli esempi vicini ad *x* abbiano la stessa label
- facendo così ci aspettiamo che la differenza tra *y* e *y_segnato* sia esattamente *w*, quindi se *w* è ragionevolmente piccolo questo non dovrebbe avere un grosso effetto nella classificazione
- la derivata di *w * x + b* rispetto ad *x1*
	![[Pasted image 20240320211214.png]]
	*il tasso di cambiamento è proporzionale ai pesi, ci conviene avere pesi piccoli*
- la formula *R(w, b)* è una funzione convessa e senza scalini che la rende facile da minimizzare
	![[Pasted image 20240320211639.png]]
- una formula alternativa è rappresentata dalla somma dei pesi in valore assoluto
	![[Pasted image 20240320211759.png]]
- al contrario di quello che si può pensare non vogliamo pesi a 0 perchè questo indicherebbe che la feature *d* non è usata nella classificazione
- il caso in cui vogliamo pesi a 0 è se abbiamo delle features irrilevanti che non vogliamo considerare
	![[Pasted image 20240320212000.png]]
	*1[ ] rappresenta la funzione indicatore che restituisce 1 se quello che c'è tra parentesi è vero, altrimenti 0, questa funzione conta 1 per tutti i pesi diversi da 0*
- le *p-norm* rappresentano una famiglia di norme che hanno una formula simile 
	![[Pasted image 20240320212319.png]]
	*norm 2 corrisponde alla norma euclidea, norm 1 corrisponde al valore assoluto entrambi visti in precedenza*
- al variare del valore di *p* cambia la rappresentazione grafica sul piano
	- max_norm: quadrato
	- 2-norm: cerchio
	- 1-norm: diamante
	- p < 1 norm: simile ad una stella
- per p < 1 la norma non è convessa

### Gradient Descend:
>GD è un algoritmo di ottimizzazione generico capace di individuare il valore minimo di una cost function consentendo di sviluppare un modello dalle accurate previsioni
- supponiamo di voler trovare il minimo di una funzione *f(x)*
- l'ottimizzatore mantiene una stima corrente del parametro di interesse *x*
- ad ogni step si misura il gradiente della funzione che si vuole ottimizzare
- questa misura si verifica alla location corrente *x* e si chiama gradiente *g*
- si prende una decisione nella direzione del gradiente dove la dimensione del passo è controllata dal parametro *η* (eta)
- lo step completo è *x <- x + ηg* 
- e si ripete

*il gradiente in matematica rappresenta il vettore le cui componenti sono le derivate parziali della funzione stessa rispetto agli assi cartesiani di riferimento*

![[Pasted image 20240320214022.png]]
*pseudocodice di gradient descend, K è il numero dei passi, z sono i nostrii esempi quindi in questo caso la funzione è f(z)*

![[Pasted image 20240320214434.png]]
*formula per il calcolo della derivata*

- l'ottimizzazione opera con questa formula
	![[Pasted image 20240320214624.png|200]]
- una volta che tutti i pesi saranno classificati bene la derivata tenerà a 0
- per i punti classificati male il gradiente punta nella direzione *-ynxn* per cui l'aggiornamento è della forma *w <- w +cynxn* dove la costante *c* è grande per punti classificati male e viceversa

![[Pasted image 20240320214837.png]]
*formula per il calcolo del gradiente*

- guardando alla parte del gradiente legata al regolarizzatore l'update funziona con *w <- w - λw = (1-λ)w*
- questo ha lo scopo di restringere il vettore dei pesi e farli tendere a 0 che è l'effetto del regolarizzatore

*come scegliere il passo*
- scegliere un passo troppo grande potrebbe significare superare il minimo
- un passo troppo corto invece potrebbe voler dire dover spendere troppo tempo per arrivare al minimo
![[Pasted image 20240320220240.png]]


> [!NOTE] Gradient Descent Convergence Theorem
> Sotto certe condizioni, per una dimensione del passo scelta appropriatamente costante (η1 = η2, ... = η) il tasso di convergenza di gradient descent è O(1/k).
> Più nello specifico se chiamiamo z* il minimo globale di f, abbiamo che
> ![[Pasted image 20240320220647.png]]

- ci fermiamo quando la funzione obbiettivo cambia di poco oppure quando i parametri cambiano poco
- possiamo anche implementare early stopping, controllando le performance ad ogni iterazione

#### Subgradients:
- la 1-norm non è derivabile attorno a *wd = 0* e hinge loss non è derivabile attorno *yy_segnato = 1*
- ottimizzazione del subgradiente
	- non ci si preoccupa del fatto che la derivata non esista e applicare comunque gradient descent
	- considerando la hinge function f(z) = max{0, 1, -z}
	- questa funzione è derivabile per z > 1, derivabile per z < 1 ma non derivabile per z = 1
	![[Pasted image 20240320221746.png]]
	*l'immagine mostra la derivazione per parti*

	![[Pasted image 20240320222135.png]]
	*derivazione effettuata sulla formula standard per hinge loss*

![[Pasted image 20240320222353.png]]
*pseudocodice di subgradient descent per hinge loss regolarizzata (con 2-norm regolatore)*

### Support Vector Machines (SVM):
> SVM è un algoritmo supervised che classifica i dati trovando un iperpiano ottimale che che massimizzi la distanza (grande margine) tra ogni classe, possono essere usati in classificazione e regressione
- *inductive bias:* scegliere il separatore con il margine maggiore
![[Pasted image 20240321091719.png]]
*vogliamo avere uno spazio tra l'iperpiano e il primo punto in questo caso è >=1*

- immaginiamo di avere un algoritmo che si comporta bene nei primi 9999 esempi e classifichi male il 10000esimo
- il punto viene classificato dalla parte sbagliata dell'iperpiano
- utilizziamo un parametro chiamato *slack* che ci permette di muovere il punto dalla parte giusta
- funziona bene purchè i punti da spostare siano pochi
- la capacità di muovere il punto è *ξ* (xi)
![[Pasted image 20240321092244.png]]
*l'immagine mostra come un punto della classe negativa è classificata male*

- **Soft Margin:** situazione in cui il margine tra iperpiano e punti è grande
- l'iperparametro C > 0 controlla situazioni di overfitting e underfitting
![[Pasted image 20240321092535.png]]
*c'è una variabile slack per ogni esempio e devono essere tutte 0 o maggiori*

- **Hard-Margin:** situazione in cui non c'è molto margine tra l'iperpiano e i punti perchè questi sono molto vicini
![[Pasted image 20240321093525.png]]
- la dimensione del margine è inversamente proporzionale alla norma del vettore dei pesi
- **massmizzare il margine è equivalente a minimizzare ||w||**

- anche senza avere il valore di *ξ* è possibile ricavarlo in questo modo
- valido sia per punti negativi e positivi
- il valore ottimale per *slack* non è altro che hinge loss dell'esempio corrispondente
![[Pasted image 20240321094058.png]]

- riscrivendo la funzione di SVM quindi otteniamo
![[Pasted image 20240321094340.png]]

