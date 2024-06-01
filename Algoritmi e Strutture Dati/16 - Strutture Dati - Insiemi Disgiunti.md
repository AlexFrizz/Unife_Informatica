| Lezione Precedente | [[15 - Strutture Dati - Tabelle Hash]] |
| ------------------ | -------------------------------------- |
| Lezione Successiva |                                        |
_gli appunti fanno riferimento alla lezione 17 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Insiemi Disgiunti
>Sono una struttura dati astratta le cui particolarità derivano dalle operazioni associate:
>- **MakeSet:** costruisce un nuovo insieme disgiunto
>- **Union:** unisce due insiemi in uno solo
>- **FindSet:** trova il rappresentante dell'insieme al quale appartiene l'elemento
>
>inoltre diciamo che la struttura è
>- parzialmente dinamica
>- parzialmente sparsa
>- non basata sull'ordinamento

ogni insieme è dotato di un *rappresentante*, che può essere un elemento qualsiasi, purchè si trovi in prima posizione 

possiamo immaginare S come un array che contiene tutte le chiavi e ad ogni elemento vi è collegato un puntatore ad una lista *S* che contiene tutti gli elementi di quell'insieme, il primo elemento è sempre il rappresentante, tutti puntano alla testa (che non è il primo elemento ma contiene il puntatore del primo elemento) e la testa punta alla coda

![[Pasted image 20240525095000.png]]

### Operazioni
![[Pasted image 20240525095128.png]]
questa operazione assumendo che nell'array esista già l'elemento crea una nuova lista (insieme) a partire dal nuovo elemento

![[Pasted image 20240525095232.png]]
sappiamo per definizione che il rappresentante è sempre in testa alla lista quindi restituiamo quell'indirizzo nella ricerca

![[Pasted image 20240525095317.png]]
volendo unire due insiemi colleghiamo la testa di uno dei due alla coda dell'altro in modo che il rappresentante sia solo uno 

#### Correttezza e Complessità
correttezza e terminazione non sono in discussione basta facilmente analizzare i codici

Complessità:
in questo caso per analizzare la complessità ci dobbiamo servire di una tecnica chiamata **analisi ammortizzata**, si tratta di calcolare il costo **medio** di una operazione qualsiasi nel contesto di un gruppo di operazioni piuttosto che il costo per operazione

diciamo che *m* è il numero di operazioni fatte di cui *n* MakeSet tale che *n<=m*

il caso peggiore come si intuisce avviene quando tutti gli elementi formano un insieme composto solo da se stessi quindi spendiamo *n* numero degli elementi per creare gli insiemi e abbiamo $$Θ(n)$$poi per le operazioni di union il costo aumenta esponenzialmente perchè i puntatori da spostare ad ogni unione crescono sempre (prima 1 poi 2 poi 3... fino ad n).
Il totale della complessità risulta essere $$Θ(n^2)$$ le operazioni di FindSet non contibuiscono a cambiare la struttura degli insiemi ma qualora dovessero essercene la complessità si ridurrebbe

per vedere invece il caso medio ammortizzato dividiamo il costo totale delle operazioni per il costo di una singola operazione $$\frac{Θ(n^2)}{Θ(n)}=Θ(n)$$
## Insiemi Disgiunti con unione pesata
una prima strategia per ridurre la complessità è quella di utilizzare la tecnica dell'unione pesata, in questo caso tutto rimane uguale tranne che consideriamo per ogni insieme anche il valore del rango (*rank*) ossia il numero di elementi di ciascun insieme in questo modo nella fase di union possiamo spostare i puntatori dell'insieme che contiene meno elementi

### Operazioni
![[Pasted image 20240525100946.png]]
il make set rimane invariato tranne che viene aggiunto l'attributo rank inizialmente impostato ad 1 perchè contiene un solo elemento

![[Pasted image 20240525101046.png]]
come anticipato nella union vengono spostati solo i puntatori dell'insieme che contiene meno elementi

#### Correttezza e Complessità
la correttezza e la terminazione ancora una volta non sono in dubbio

Complessità:
qui il caso peggiore avviene quando uniamo gli insiemi a due a due e tutti sono della stessa dimensione quindi non viene sfruttato l'attributo rank, in questo caso la complessità è logaritmica perchè cresce con la dimensione dell'albero e quindi $$Θ(log(n))$$per fare un esempio abbiamo n/2 unioni alla volta quindi:
- n/2 la prima unione (otteniamo n/2 insiemi di due elementi)
- n/4 la seconda unione (otteniamo n/4 insiemi di quattro elementi)
- e così via

coniderando ora anche l'operazione di make set ancora una volta ipotizziamo di dover creare tutti gli insiemei con un solo elemento, la complessità diventa quindi $$Θ(n*log(n))$$
## Insiemi Disgiunti con Foreste di Alberi
in questo caso la rappresentazione degli insiemi è basata su alberi e non su liste.
un nodo *x* (un elemento) contiene le seguenti informazioni
- x.p (il padre)
- x.rank (un limite superiore all'altezza del sotto-albero radicato in x)

![[Pasted image 20240525160745.png]]
nell'esempio possiamo distinguere le seguenti caratteristiche
- ad ogni nodo è accoppiato il valore di chiave e il valore di rango
- i puntatori vanno verso l'alto quindi i figli puntano i padri invece del contrario

### Operazioni
**MakeSet:**
![[Pasted image 20240525161953.png]]
l'operazione dunziona alla stessa maniera solo che in questo caso viene creato un albero e la sua altezza massima è 0 perchè vi è presente un solo elemento

**Union:**
![[Pasted image 20240525162006.png]]
in questo caso le fasi sono due, si trovano i rappresentanti, che sono anche le radici dei due alberi *x* e *y* poi si confronta il rango e si aggiorna solo il padre rendendolo uguale all'altro elemento

**FindSet:** 
![[Pasted image 20240525161940.png]]
questa operazione diventa attiva, non solo come sempre restituisce il rappresentante ma scorre i puntatori e li aggiorna collegandoli al rappresentante (*compressione del percorso*)
![[Pasted image 20240525161809.png]]

#### Correttezza e Complessità
di nuovo correttezza e terminazione non sono in discussione

Complessità:
l'operazione union in questo caso aggiorna solo al massimo un puntatore, l'operazione FindSet invece aggiorna un certo numero di puntatori ma lo fa una sola volta, e poi non vengono più toccati (costerà poi un tempo costante) 

diciamo quindi saltando la dimostrazione che la crescita di una certa funzione *a* è estremamente lenta e una corretta analisi di *m* operazioni darebbe che il costo totale è $$O(m * a(n))$$che in ogni situazione pratica diventa $$O(m)$$
