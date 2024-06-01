| Lezione Precedente | [[22 - Strutture Dati - Grafi, visita in profondità]] |
| ------------------ | ----------------------------------------------------- |
| Lezione Successiva | [[24 - Strutture Dati - Grafi e percorsi minimi]]     |
_gli appunti fanno riferimento alla lezione 27 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Alberi di copertura minima
analizziamo qui grafi pesati indiretti e connessi 
>Definiamo un albero di copertura minima (MST) come un sottoinsieme di archi che forma un albero (indiretto) che copre tutti i vertici e la cui somma dei pesi è minima

analizziamo il problema con una strategia di tipo greedy ovvero, faremo la scelta localmente migliore e vogliamo in questo modo ottenere il risultato globalmente migliore

In un MST quindi eliminiamo qualunque arco da un albero di copertura otteniamo due alberi sconnessi
in generale qualunque partizione di *V* in due sottoinsiemi *S* e *V\S* viene chiamata *taglio del grafo*, costruire un MST significa considerare un taglio ed aggiungere progressivamente un arco 

### Algoritmo di Prim
l'idea alla base è quella di partire da un vertice qualsiasi e ad ogni passo aggiungere un arco (ed un vertice) di peso minimo in modo che l'arco aggiunto sia un arco sicuro

ogni vertice di G viene arricchito con due campi: 
- *v.key*, il peso minimo inizialmente infinito tra gli archi che connettono qualche vertice di T con *v* 
- *v.π* il padre di v, inzialmente Nil, nel MST risultante

inizialmente tutti i vertici si trovano nella coda di priorità Q semi-ordinata su *v.key* 

![[Pasted image 20240527223835.png]]
la procedura funziona come segue:
- inizialmente tutti i vertici hanno chiave infinito e nessun padre
- viene inizializzata la coda (con tutti i vertici) e fintanto non è vuota si esegue il ciclo while
- viene estratto il minimo (inizialmente la radice che ha peso 0)
- si esegue un ciclo for su tutti i vertici adiacenti 
- fintanto che il valore del peso del ramo è inferiore al valore di chiave del nodo successivo allora si salva il puntatore al padre e si aggiorna (DecreaseKey) il valore (finora) minore di peso per quel ramo
![[Pasted image 20240527224315.png]]

#### Correttezza e Complessità
Correttezza:
definiamo *T* come l'insieme di tutte le coppie *(v.π, v)* tali che *v.π* è definito e *v ∈/ Q.*
T è sempre sottoinsieme di qualche MST 

Complessità:
per analizzare la complessità bisogna definire se il grafo è 
- sparso
- denso

e se la struttura della coda sia basata su
- heap
- array

la scelta migliore è con code di priorità basate su heap e tipicamente con grafi sparsi la complessità che ne risulta dall'analisi aggregata è $$Θ(|E| · log(|V|))$$questo perchè è il costo massimo delle operazioni di decremento ed estrazione del minimo $$Θ(log(|V|))$$per il numero di archi del MST

### Algoritmo di Kruskal
l'idea qui è che possiamo ordinare gli archi in ordine crescente di peso e analizzandoli uno ad uno in questo ordine, stabilire se inserirlo come parte dell'albero di copertura minima oppure no 

un arco *(u, v)* non è parte di nessun MST se *u* e *v* sono già connessi da qualche altro arco precedentemente scelto

dati gli archi di T e dato un insieme V di vertici diciamo che un sottoinsieme S di V è T-connesso se considerando solo archi in T è un albero ed è massimale

diciamo che tutte le componenti T-connesse che fanno parte di un sottoinsieme S danno vita ad un taglio generalizzato 

in questo caso ci serviamo della struttura per insiemi disgiunti con le seguenti operazioni:
- *MakeSet:* costruisce un nuovo insieme (componente T-connessa)
- *FindSet:* stabilisce se due elementi appartengono allo stesso insieme (componente T-connessa)
- *Union:* unisce due insiemi in uno solo (due componenti T-connesse in una sola, come conseguenza di aver scelto un arco)

![[Pasted image 20240527232320.png]]
![[Pasted image 20240527234505.png]]
#### Correttezza e Complessità
Correttezza:
l'invariante è che ogni volta che viene inserito un nuovo arco, è un arco sicuro, questo perchè li abbiamo ordinati e quindi è sicuramente quello di peso minore

Complessità:
immaginiamo l'implementazione in un grafo sparso e con insiemi disgiunti (con foreste di alberi) abbiamo quindi che l'operazione di inizializzazione è costante, e l'ordinamento costa $$Θ(|E| · log(|E|)$$le altre operazioni hanno un costo minore quindi la complessità rimane quella dell'ordinamento