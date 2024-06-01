| Lezione Precedente | [[02 - Correttezza e complessità di Algoritmi Iterativi]] |
| ---- | ---- |
| Lezione Successiva | [[04 - Notazione Asintotica]] |

_gli appunti fanno riferimento alla lezione 2 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

l'algoritmo che andremo ad analizzare nel caso ricorsivo sarà RecursiveBinarySearch: 
- il funzionamento consiste nel cercare un elemento k in un array A la cui dimensione è compresa tra low e high (indici), dividendo ad ogni chiamata ricorsiva metà dell'array

![[Screenshot 2024-02-03 202246.png]]
*nil è una parola chiave che indica una locazione di memoria vuota*

### Analizziamo l'algoritmo in relazione alle proprietà fondamentali: 
- **Terminazione:** il controllo (*low > high*) e (*A[mid] = k*) ci assicurano una terminazione in quanto condizioni di uscita dalla funzione
- **Correttezza:** anche per quanto riguarda gli algoritmi ricorsivi utilizziamo la tecnica dell'invariante (induttiva), solo che non avendo cicli ai quali affidarci in questo caso ci concentriamo su ciò che accade dopo ogni chiamata ricorsiva
	- in questo caso se k è in A allora si trova certamente tra *A[low, ..., ligh]*
- **Completezza:** essendo RecursiveBinarySearch un algoritmo noto diamo per assodata la completezza

#### Studiamo ora la complessità dell'algoritmo: 
considerazioni:
*è più complicato nel caso ricorsivo, calcolare il costo delle istruzioni semplici è inutile perchè le istruzioni prima delle chiamate ricorsive sono costanti e non ci sono cicli*
- la nostra funzione *T(n)* è un incognita in questo caso, perchè non sappiamo che peso dare alle chiamate ricorsive
- sappiamo certamente che prima della prima chiamata ricorsiva il costo è pari ad una costante *c* che approssimiamo ad 1

##### Caso peggiore:
- in questo caso la chiamata ricorsiva viene effettuata esattamente una volta (perchè si distingue il caso in cui *A[mid]>k oppure A[mid]<k*) e al massimo sulla metà degli elementi originali più uno (perchè nella chiamata ricorsiva vengono passati i valori *mid+1, high oppure low, mid-1* che rappresentanoo esattamente la metà dell'array precedente)
- diciamo quindi che la complessità della chiamata ricorsiva è pari a un incognita *T(n/2)*

![[Screenshot 2024-02-05 174831.png]]
*si prende solo il floor della divisione n/2 quindi la parte intera*

##### Caso migliore:
pur non essendo informativo analizziamo il caso migliore: 
- in questo caso la chiamata ricorsiva non avviene mai perchè *A[mid]* corrisponde al valore *k* che si vuole cercare
- al controllo *if(A[mid] = k)* si esce dal ciclo e si ritorna il valore di mid
- ? la complessità quindi non avendo chiamate ricorsive corrisponde ad una costante ?

conclusioni:
- per analizzare questi algoritmi è necessario imparare la notazione asintotica 

