| Lezione Precedente | [[01 - Intuizione Algoritmica]]                           |
| ------------------ | --------------------------------------------------------- |
| Lezione Successiva | [[03 - Correttezza e complessità di Algoritmi Ricorsivi]] |
_gli appunti fanno riferimento alla lezione 2 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

#### Caratteristiche fondamentali di un Algoritmo:
- **Correttezza:** cioè che restituisce sempre la risposta corretta
- **Completezza:** significa che ogni risposta corretta è prima o poi restituita
- **Terminazione:** significa assicurarsi che per ogni input la computazione finisca 

>riassumiamo queste 3 proprietà riferendoci solo a Correttezza

>[!LIGHT] Definizione di Complessità:
>La complessità di un algoritmo definisce in termini matematici il numero di passi che sono necessari per eseguire l'algoritmo in termini del numero di oggetti che costituisce l'input


### Analizziamo ora le caratteristiche degli Algoritmi Iterativi (con cicli)
*prendendo in esame l'algoritmo Insertion Sort*
```
proc InsertionSort(A)
{
	for(j=2 to A.length)
	{
		key = A[j]
		i = j - 1
		while((i > 0) and (A[i] > key))
		{
			A[i+1] = A[i]
			i = i - 1
		} 
		A[i+1] = key
	}
}
```
*l'algoritmo si occupa di ordinare un array A fornito in input*

#### Verifichiamo ora le 3 proprietà degli algoritmi:
- **Terminazione:** le condizioni di esecuzione di InsetionSort sono due:
	- il ciclo principale (for) si esegue fino a quando *j* non raggiunge le dimensioni dell'array A, quindi non sono stati controllati tutti i valori
	- il ciclo più interno (while) si esegue fintanto che *i* è positiva (*i > 0*)
- **Correttezza:** per verificare questa proprietà utilizziamo la tecnica dell'invariante, ovvero stabilire una proprietà del ciclo principale (o di un ciclo dell'algoritmo) che sia vera prima della prima esecuzione, dopo ogni esecuzione e dopo l'ultima esecuzione
	- nel caso di InsertionSort l'invariante è che *A[1, ..., j-1]*  è sempre ordinato in maniera non crescente
- **Completezza:** essendo InsertionSort un algoritmo noto e ben definito è dato per assodato che sia anche completo

#### Studiamo ora la complessità dell'algoritmo Iterativo:
>all'atto pratico la complessità di un algoritmo iterativo si studia sommando la complessità di ogni singola istruzione semplice

![[Screenshot 2024-02-03 180010.png]]
considerazioni:
- tutti i pesi sono moltiplicati per una costante (incognita) che dipende da fattori esterni (es. prestazioni della macchina)
- la prima istruzione vale *n * c1* perchè *j* parte da 2 ma arriva fino ad *A.length + 1* condizione necessaria per la terminazione (verifica tutti gli n elementi dell'array)
- le istruzioni dentro al ciclo esterno valgono *( n - 1)*c* perchè *j* parte da 2 e *A.length + 1* non viene mai eseguita
- *tj* rappresenta il numero di volte che il ciclo interno è eseguito
- il costo dell'istruzione *while* è l sommatoria da *j=2* ad *n* di *tj* dove (*tj* va da 2 ad *n*)
- il costo delle istruzioni dentro il ciclo più interno è la sommatoria da *j=2* ad *n* di (*tj-1*) perchè *j* comincia da 2

##### Caso Migliore:
- il caso migliore è quello in cui il nostro array A in input sia già ordinato in questo caso avvengono le seguenti cose:
	- il ciclo interno non viene mai eseguito (viene eseguita l'istruzione while ma non si entra nel ciclo) perchè *tj=1*
	- *tj=1* allora le istruzioni del ciclo interno hanno *(tj-1)=0* quindi non vengono mai eseguite
	- la complessità di while è quindi (*c4 * (n - 1)*) perchè il ciclo viene eseguito una sola per ogni elemento dell'array *A-1* perchè l'indice iniziale è *j=2*
	
![[Screenshot 2024-02-03 184156.png]]
Risolta otteniamo una funzione lineare per qualche costante a, b:
- *T(n) = an + b*

##### Caso Peggiore:
- il caso peggiore è quello in cui il nostro array A in input sia ordinato in ordine decrescente (inverso a quello di InsertionSort), avvengono quindi le seguenti cose:
	- ad ogni istanza del ciclo più esterno (for) il ciclo più interno viene eseguito *j* volte e le istruzioni interne *j-1* volte (sapendo che *tj* è il numero di volte che viene eseguito il ciclo interno e che *tj = j*)
	- la sommatoria relativa al while e le due somatorie delle due istruzioni interne si possono ricondurre a delle sommatorie di gauss per semplicità
	- anche in questo caso l'istruzione del ciclo while viene eseguita un numero di volte -1
	
![[Screenshot 2024-02-03 185551.png]] ![[Screenshot 2024-02-03 185602.png]]
*fig1: costo istruzione while*                                   *fig2: costo istruzioni interne*

![[Screenshot 2024-02-03 185626.png]]
Risolta otteniamo, una funzione quadratica per qualche a, b, c:
- *T(n) = an^2 + bn + c*

##### Conclusioni:
- diciamo che la complessità è data da T(n) = Θ(f(n)) ed f(n) si ottiene eliminando tutti i termini di ordine inferiore al maggiore e tutte le sue costanti, pertanto
	- nel caso migliore la complessità è: Θ(n)
	- nel caso peggiore la complessità è: Θ(n^2)
	- ? nel caso medio la complessità è: Θ(n^2/2) ?


