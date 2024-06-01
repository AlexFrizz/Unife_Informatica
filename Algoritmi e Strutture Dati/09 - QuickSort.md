| Lezione Precedente | [[08 - Laboratorio 0]] |
| --- | --- |
| Lezione Successiva | [[10 - Laboratorio 1]] |
_gli appunti fanno riferimento alla lezione 7 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

#### Struttura di QuickSort:
QuickSort è un algoritmo che prevede qualche analogia con MergeSort, come ad esempio di essere di tipo *divide and conquer*, il fatto di essere ricorsivo e avere due procedure:
- **Partition:** questa procedura prende in input l'array *A* il suo indice minimo *p* e il suo indice massimo *r*, *r* viene sempre considerato come *pivot* successivamente vè un ciclo che sposta a sinistra di *i+1* gli elementi più piccoli di *r* e sposta la finestra avanti in modo che quelli più grandi siano sempre compresi tra *i+1* e *j*
- **QuickSort:** comprende l'implementazione di Partition e prevede due chiamate ricorsive (sinistra e destra del pivot)
![[Pasted image 20240214170909.png|300]]![[Pasted image 20240214171011.png|300]]
#### Correttezza di Partition:
per poter analizzare la correttezza di QuickSort è prima necessario analizzare la correttezza di Partition:
- **Terminazione:** avendo un ciclo for abbiam anche una condizione di terminazione, il ciclo infatti si eseguirà fino a quando l'indice *j* non raggiungerà l'indice *r*
- **Correttezza:** ovviamente ricorriamo al metodo dell'invariante e lo attribuiamo all'unico ciclo presente, il *for*; le proprietà sempre vere sono:
	- se *p<=k<=i* allora *A[k]<=x* (se l'elemento sta a sinistra di *i* allora è più piccolo del pivot)
	- se *i+1<=k<=j-1* allora *A[k]>x* (se l'elemento sta compreso tra *i+1* e *j-1* allora è maggiore del pivot)
	- se *k=r* allora *A[k]=r*
- **Completezza:** essendo QuickSort un algoritmo noto possiamo dare per assodato che sia anche completo

#### Correttezza di MergeSort:
analizziamo ora la correttezza dell'algoritmo nella sua interezza:
- **Terminazione:** la condizione di terminazione di MergeSort è rappresentata dal controllo *if(p<r)* le chiamate ricorsive invece vengono effettuate solo fintanto che gli indici hanno senso (il minore è più piccolo non strettamente del maggiore), una volta che questa condizione non è più soddisfatta il controllo viene restituito al chiamante
- **Correttezza:** l'algoritmo è corretto per due ragioni:
	- Partition è corretto
	- sappiamo inoltre che con la tecnica dell'invariante induttiva per le chiamate ricorsive dopo ogni chiamata di QuickSort l'array sarà ordinato perchè quando le chiamate ricorsive terminano e il controllo verrà restituito al chiamante, l'array sarà stato scomposto in array di dimensione 1 e quindi ordinati anche una volta riunito l'array
- **Completezza:** essendo QuickSort un algoritmo noto possiamo dare per assodato che sia anche completo

#### Complessità di Partition:
la complessità di Partition risulta semplice da analizzare
- avendo un unico ciclo (*for*) essendo tutte le altre istruzioni costanti risulta che la sua complessità è: $$Θ(n)$$

#### Complessità di MergeSort:
ora che abbiamo facilmente analizzato la complessità di partition non è altrettanto facile calcolare quella di MergeSort
*MergeSort è uno di quegli algoritmi in cui ha senso considerare il caso ottimo, medio e pessimo*
##### Caso Peggiore:
la situazione che ci porta a questo caso è quella in cui si ha una *partizione sbilanciata* ovvero quando si ha un pivot tale che ad ogni passo genera un array di dimensione *0* e uno di dimensione *n-1* quindi tutta la partizione meno l'elemento pivot
- degli esempi possono essere quando il pivot è l'elemento più piccolo o più grande di tutti 
- **ad esempio quando l'algoritmo è già ordinato** 
![[Pasted image 20240214195215.png|300]]

- la ricorrenza in questo caso è quindi:
	![[Pasted image 20240214194936.png]]
- la cui soluzione, vista come sviluppo si una serie aritmetica è evidentemente: $$T(n) = Θ(n^2)$$

##### Caso Migliore:
analizziamo ora il caso migliore di MergeSort che ci porta in una soluzione in cui la partizione è sempre bilanciata nel modo più equo possibile
- in questo la ricorrenza che otteniamo è:
	![[Pasted image 20240214195728.png]]
	- il *2* rappresenta il numero di chiamate ricorsive
	- il *T(n/2)* il numero degli elementi su cui avviene la chiamata
	- *Θ(n)* la complessità di partition quindi il peso che ha ogni chiamata ricorsiva
- risolvendo questa facile ricorrenza con il *master theorem* otteniamo la seguente complessità: $$Θ(n*log(n))$$
##### Caso Qualsiasi:
per analizzare il caso medio è prima necessario analizzare un caso qualsiasi ovvero una situazione in cui il nostro pivot si trovi in una posizione diversa dal caso migliore o peggiore
- esempio in cui il pivot è il penultimo elemento dell'array: 
	![[Pasted image 20240214200707.png]]
- se risolviamo questa ricorrenza possiamo notare che ancora una volta la complessità risulta: $$O(n*log(n))$$
- notiamo che la complessità è identica al caso migliore in quanto la partizione è costante (nessuna delle due è *0*) allora non è sbilanciata
- questa considerazione tuttavia non ci da la certezza di non trovarci mai in una partizione sbilanciata, infatti:
	- *per ogni scelta del pivot esiste un input tale che porterà la corrispondente versione di QuickSort al caso peggiore*
	- *ma qual'è la probabilità che questo accada?*

##### Caso Medio:
sotto l'ipotesi che tutti gli input siano equamente probabili otterremo anche in questo caso la stessa complessità:
per garantire la precedente ipotesi possiamo costruire una versione randomizzata dell'algoritmo 
![[Pasted image 20240214201547.png|300]]![[Pasted image 20240214201634.png|400]]
- denotiamo come *k* la partizione sinistra ed *(n-1)-k* la partizione destra escludendo quindi il pivot qualsiasi sia
- se le partizioni sono ugualmente probabili hanno quindi tutte *1/n* 
	![[Pasted image 20240214202144.png]]
	formalizzata:
	![[Pasted image 20240214202212.png]]
- dimostriamo quindi che come abbiamo già detto la complessità sarà $$O(n*log(n))$$ tramite sostituzione, e in particolare mostriamo che $$T(n)<=a*n*log(n)+b$$
	![[Pasted image 20240214202439.png]]
	![[Pasted image 20240214202548.png]]
	![[Pasted image 20240214202623.png]]

- come notiamo ci troviamo nella forma di parenza abbiamo quindi dimostrato che la complessità di QuickSort nel caso medio corrisponde a $$O(n*log(n))$$

#### Conclusioni:
diamo infine alcune proprietà aggiuntive agli algoritmi che abbiamo studiato fino ad ora
- **in place:** un algoritmo si dice in place se non occupa memoria aggiuntiva per la sua esecuzione
- **stabile:** significa che tutti gli elementi uguali rimangono nelle stesse posizioni relative

|  | In Place | Stabile |
| :--- | :--: | :--: |
| InsertionSort | X | X |
| MergeSort |  | X |
| QuickSort | X |  |

