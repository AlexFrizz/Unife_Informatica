| Lezione Precedente | [[03 - Correttezza e complessità di Algoritmi Ricorsivi]] |
| ------------------ | --------------------------------------------------------- |
| Lezione Successiva | [[05 - Soluzione di Ricorrenze]]                          |

_gli appunti fanno riferimento alla lezione 2 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

le notazioni che andremo a studiare sono:
- *O( )* limite dall'alto
- *Ω( )* limite dal basso
- *Θ( )* 
- in maniera minore anche *ω( )* e *o( )*
- *n0* rappresenta il punto iniziale da cui comincia il limite dall'alto o dal basso

#### Notazioni *O(n)* e *Ω(n)*:

- nello specifico diciamo che *f(n)* è limitata dall'alto ( *O(g(n))* ) se esiste una costante *c>0* t.c. per tutti gli *n>=n0* allora:
	![[Screenshot 2024-02-06 161555.png]]

- analogamente diciamo che  *f(n)* è limitata dal basso ( *Ω(g(n))* ) se esiste una costante *c>0* t.c. per tutti gli *n>=n0* allora:
	![[Screenshot 2024-02-06 161657.png]]

- effettuando la maggiorazione di un polinomio di grado *k* (qundi andando a sostituire a tutti gli *n* l' *n* di grado maggiore quindi *n^k*) allora otteniamo che il polinomio precedente è uguale a quello maggiorato per una costante *c* (*c * n^k*) che è esattamente uguale ad *O(n^k)*
	![[Screenshot 2024-02-06 163511.png]]


- allo stesso modo possiamo dire che effettuando la minorazione di un polinomio di grado *k* ne risulterà lo stesso polinomio dato da (*c * n^k*) che è esattamente uguale a *Ω(n^k)*
	![[Screenshot 2024-02-06 163426.png]]

>non tutte le funzioni sono limitate dall'alto e dal basso dalla stessa funzione, questo vale solo per i polinomi

a conferma di ciò immaginiamo di avere *f(n) = log(n)*
- essendo *log* una funzione che cresce in maniera esponenziale è vero che potremmo avere un *O(n)* come limite dall'alto
- ma non è invece altrettanto vero che *log(n) = Ω(n)* prendiamo un ipotetico grafico di funzione come esempio
	![[Screenshot 2024-02-06 164405.png]]

#### Notazione *Θ(n)*:

> la notazione *Θ(g(n))* indica che la nostra funzione *f(n)* è sia limitata dall'alto da un *O(g(n))* e limitata dal basso da un *Ω(g(n))* 

> [!NOTE] Definizione Notazione Asintotica
> per una funzione *f(n)*, diremo che *f(n)* è dello stesso ordine di grandezza di *g(n)* se e solo se esistono due costanti *c1, c2 > 0* t.c per qualche n0, per tutti gli *n >= n0* è il caso che:
> ![[Pasted image 20240206170010.png]]
> quindi asintoticamente *f(n)* di comporta come *g(n)* denotato come *f(n) = Θ(g(n))*

![[Screenshot 2024-02-06 170715.png]]
*definiamo f(n) uguale ai limiti O(n) o Ω(n) oppure uguale a Θ(n) perchè ipotizzando di aumentare sempre di più la scala del grafico i limiti diventerebbero sempre più simili ad f(n) in maniera indistinguibile*

#### Notazioni *o(n) e ω(n)*:

- per confrontare ordini di grandezza di funzioni diverse usiamo le notazioni *o piccolo e omega piccolo*
- diremo quindi *f(n) = o(g(n))* (è di ordine inferiore a) se e solo se per ogni costante *c>0* esiste una notazione *n0* t.c. per ogni *n>n0* si verifica che *0 <= f(n) <= c * g(n)* 
- diremo quindi *f(n) = ω(g(n))* (è di ordine superiore a) se e solo se per ogni costante *c>0* esiste una notazione *n0* t.c. per ogni *n>n0* si verifica che *0 <= c * g(n) <= f(n)* 

>puntualizziamo che nella definizione di *O()* e *Ω()* la funzione *f(n)* può prima o poi avvicinarsi o allontanarsi dai limiti, mentre il comportamento di *ω( )* e *o( )* prevede che non si avvicinino mai ad *f(n)* e la distanza dalla funzione deve sempre crescere

conclusioni: 
- quando andremo a definire la complessità di un algoritmo definiamo il caso che stiamo analizzando e utilizziamo le varie notazioni per darne una definizione 
- specificare la forma dell'input quando questo ha senso, non significa riuscire a specificare esattamente lo spazio di funzione alla quale la complessità appartiene




| Notazione   | Proprietà                            |
| ----------- | ------------------------------------ |
| o( ) e ω( ) | Transitiva                           |
| O( ) e Ω( ) | Transitiva + Riflessiva              |
| Θ( )        | Transitiva + Riflessiva + Simmetrica |
