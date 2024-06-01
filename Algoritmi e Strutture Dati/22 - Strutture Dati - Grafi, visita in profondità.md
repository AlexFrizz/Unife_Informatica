| Lezione Precedente | [[21 - Strutture Dati - Grafi, visita in ampiezza]]          |
| ------------------ | ------------------------------------------------------------ |
| Lezione Successiva | [[23 - Strutture Dati - Grafi e alberi di copertura minima]] |
_gli appunti fanno riferimento alla lezione 26 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_


## Grafi: Visita in Profondità e problemi collegati
>per questo tipo di visita non ci preoccupiamo se il grafo è pesato o no e nemmeno se è diretto o no vediamo tuttavia tre problemi in cui tipicamente il grafo applicato è di tipo *diretto*
>- stabilire se G è ciclico
>- costruire un ordinamento topologico di G
>- conoscere ed enumerare le componenti fortemente connesse di G

La caratteristica principale della visita in profondità è che i vertici vengono scoperti il prima possibile a partire da quelli già scoperti ed è inerentemente ricorsiva

### DepthFirstSearch
Vediamone alcune caratteristiche:
- anche in questa procedura come per la visita in ampiezza vengono utilizzati i colori (bianco, grigio e nero)
- si assume che G sia rappresentato con liste di adiacenza, e riempie un campo *v.π* per generare un albero di visita in profondità
- *v.d* rappresenta il momento in cui il vertice viene scoperto, *v.f* il momento nel quale il vertice viene abbandonato e vengono tipicamente chiamati tempi *v.d<v.f*
- al contrario della visita in ampiezza qui l'output è una foresta di alberi, uno per ogni sorgente (ogni sotto-grafo connesso di G)

![[Pasted image 20240527200552.png]]
![[Pasted image 20240527200615.png]]
la procedura esegue le seguenti operazioni in ordine:
- con un primo ciclo for tutti i vertici vengono impostati con colore bianco e senza puntatore al predecessore
- il secondo ciclo, il while, chiama chiama la procedura DepthVisit qualora il nodo sia bianco
	- si incrementa la variabile tempo e si salva e il colore del vertice diventa grigio perchè si è scoperto
	- un ciclo for si esegue per ogni vertice adiacente
		- qualora il nodo sia bianco si chiama ricorsivamente la procedura DepthVisit e si salva il puntatore al predecessore
	- terminato il for si incrementa il tempo e si salva, si imposta il vertice corrente di colore nero e si ritorna il controllo al chiamante fino a quando non ci sono più chiamate in sospeso

![[Pasted image 20240527202126.png]]
*ovviamente nella visita del grafo in questo caso essendo diretto si seguono le direzioni*

#### Complessità
la complessità in questo caso è semplice da calcolare, il primo ciclo per l'inizializzazione di tutti i vertici costa ovviamente $$Θ(|V|)$$poi con il secondi ciclo vengono percorsi un certo numero di rami in base al colore e alla direzione dei rami quindi il costo è $$Θ(|E|)$$
tramite l'analisi aggregata otteniamo che la complessità totale è $$Θ(|V| + |E|)$$
### Cicli
una delle applicazioni di DepthFirstSearch è determinare se un grafo diretto è privo o no di cicli, 
definiamo come *ciclo* un percorso *v1, v2,..., vk* di vertici tale che per ogni *i* esiste l'arco (*v_i, v_i+1*) e che *v1=vk*
![[Pasted image 20240527203518.png]]![[Pasted image 20240527203537.png]]
la procedura funziona esattamente come DepthFirstSearch solo che (come indicato nell'immagine) vi è un aggiunta
- al ritorno della procedura se il vertice è di colore grigio, allora siamo certamente in un ciclo 

### Ordinamento Topologico
qualora il grafo non sia ciclico ha senso parlare anche di ordinamento topologico, immaginiamo di avere un grafo diretto G dove un arco *(u, v)* rappresenta che *u* deve essere posto prima di *v*

in questo problema quindi si prende in input un grafo connesso G diretto senza cicli e si restituisce una lista collegata di vertici topologicamente ordinati

![[Pasted image 20240527205014.png]]
![[Pasted image 20240527205030.png]]
la procedura quindi funziona come segue: 
- come al solito tutti i vertici vengono inizializzati e si chiama la procedura DepthVisitTS per ogni vertice bianco 
- vengono aggiornate le informazioni sul tempo e il colore del vertice corrente
- per ogni nodo adiacente avviene una chiamata ricorsiva a DepthVisitTS 
- quando il controllo torna al chiamante oltre ad aggiornare tempo e variabile, il vertice viene inserito in una Lista 

l'ordine di inserimento dipende ovviamente dalla direzione dei rami del grafo
![[Pasted image 20240527205753.png]]

#### Correttezza e Complessità
Correttezza:
dopo l'esecuzione, la lista di uscita è topologicamente ordinata

la complessità è la stessa della visita in profondità e la terminazione è ovvia

### Componenti Fortemente Connesse (SCC)
un altra applicazione della visita in profondità è quella di determinare se un grafo ha dei vertici strettamente connessi cioè che esiste un percorso orientato in entrambe le direzioni tra un gruppo di vertici

per poter analizzare un SCC si può trasporre il grafo G invertendo la direzione di ogni arco, facendo questo i vertici risulterebbero comunque fortemente connesse
![[Pasted image 20240527210433.png]]
la procedura funziona come segue:
- la prima parte è sempre uguale, otteniamo quindi il valore di v.f per ogni vertice
- poi ripetiamo l'inizializzazione, e creiamo una lista nella quale andranno inseriti i vertici del grafo trasposto

#### Correttezza e Complessità:
Correttezza:
possiamo ricavare il suo grafo delle componenti connesse G_SCC semplicemente considerando tutti i vertici di ogni SCC di G come un unico vertice di G_SCC , e impostando che esiste un arco (u, v) in G_SCC se e solo se esiste un arco in G da uno dei nodi simboleggiati da u a uno dei nodi simboleggiato da v.

la complessità è la stessa della visita in profondità e la terminazione è ovvia