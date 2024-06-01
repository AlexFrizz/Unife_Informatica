| Lezione Precedente | [[04 - Notazione Asintotica]] |
| --- | --- |
| Lezione Successiva | [[06 - Esercitazione 1]] |
_gli appunti fanno riferimento alla lezione 2 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

**distinguiamo subito 3 forme di ricorrenza:**
- *T(n) = n^2 + 3n + 5*  -------------> Forma Esplicita e Precisa (non si usa mai perchè dipende dalla macchina)
- *T(n) = an^2 + bn*      -------------> Forma Esplicita e Generica (quasi sempre)
- *T(n) = Θ(n)*                -------------> Forma Asintotica (risultato di un analisi)

**metodi di soluzione di ricorrenze:**
- metodo dell'albero di ricorsione (o sviluppo in serie)
- master theorem per le ricorrenze
- il metodo della sostituzione (o induzione)

#### Metodo dello Sviluppo in Serie (albero):
- si basa nello sviluppare la ricorrenza per poi estrarne il comportamento
- andiamo ad applicare la ricorrenza alla ricorrenza stessa, un numero di volte pari a *floor(log(n))* 
	![[Pasted image 20240207104600.png]]
	*utilizziamo come esempio la complessità dell'algoritmo RecursiveBinarySearch*
- una volta applicata la ricorrenza mi ritroverò con un elemenro *T(1)* che rappresenta il costo finale dell'algoritmo, *T(n) = 1*
- nel nostro esempio quindi abbiamo, *1 + floor(log(n)) = Θ(log(n))*

*andando a risolvere altre ricorrenze ci accorgiamo che tutte hanno una struttura simile e che quindi è possibile andare a standardizzare un metodo standard, questo è il master theorem*
#### Master Theorem:
- la forma generale che otteniamo è questa: 
	![[Pasted image 20240207113540.png]]
- immaginando che *n* sia una potenza esatta si *b* otteniamo:
	![[Pasted image 20240207113758.png]]
- per ottenere una forma prettamente asintotica dobbiamo osservare il comportamento di *f(n)* generalizzando distinguiamo 3 casi:
	- **Caso1:** *f(n)* è di ordine polinomicamente inferiore a n^log(a) questo significa che *f(n)* è limitata dall'alto da un *O(n^log(a))* e da come risultato *Θ(n^log(a))*![[Pasted image 20240207114358.png]]![[Pasted image 20240207114916.png]]![[Pasted image 20240207115042.png]]
	- **Caso2:** se invece avessimo *f(n) = Θ(n^log(a))*: ![[Pasted image 20240207115836.png]]![[Pasted image 20240207115939.png]]
	- **Caso3:**  se invece avessimo *f(n) = Ω(n^log(a)+ε)* cioè *f(n)* è polinomicamente di grado superiore a *n^log(a)* che implica *f(n) = Ω(n^log(a))*
	- immaginiamo anche che *a * f(n/b) <= c * f(n)* per qualche *c < 1* e tutti gli *n >= n0* ![[Pasted image 20240207120632.png]]![[Pasted image 20240207120710.png]]![[Pasted image 20240207120734.png]]

*possiamo riassumere il master theorem con questo schema:*
![[Pasted image 20240207121112.png]]

*!!! Il master theorem non copre tutti i casi di ricorrenza, ma solo quelli che hanno la forma iniziale descritta in precedenza (fig. evidenziata)*

#### Metodo di Sostituzione:
- quando siamo in una forma diversa da quella del master theorem è possibile utilizzare sostituzione
- spesso sviluppo in serie e sostituzione si utilizzano assieme per avere una notazione più precisa
- il metodo si basa nel trovare per ipotesi un *O( )* oppure un *Ω( )* e poi verificarla per induzione
![[Pasted image 20240207130519.png]]
- da qui si ottiene per induzione *O(n * log(n))*
*nell'esempio scegliamo volutamente di non risolvere la ricorrenza con il master theorem per scopi didattici*

- se volessimo utilizzare la notazione Theta allora dovremmo applicare sostituzione sia per il limite dall'alto che limite dal basso

**Conclusioni:**
- come strategia generale possiamo pensare di applicare per primo il master theorem (se non funziona o la forma non lo permette) ...
- sviluppiamo in serie ...
- se necessario verifichiamo il risultato con sostituzione
