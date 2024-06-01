| Lezione Precedente | [[23 - Strutture Dati - Grafi e alberi di copertura minima]]            |
| ------------------ | ----------------------------------------------------------------------- |
| Lezione Successiva | [[25 - Strutture Dati - Grafi e percorsi minimi tra coppie di vertici]] |
_gli appunti fanno riferimento alla lezione 28 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Grafi e percorsi minimi con sorgente singola
in questa lezione vediamo la ricerca dei percorsi minimi con *grafi pesati e diretti*

la condizione per cui la definizione di percorso minimo verrebbe meno è il caso in cui avessimo cicli con pesi negativi, questo perchè ad ogni passaggio in quel ciclo il peso totale del percorso diminuirebbe e questo non sarebbe possibile

per quanto riguarda rami con pesi negativi di tipo aciclico questi non creano nessun tipo di impedimento o problema (a patto che l'algoritmo scelto riesca a gestirli)

la lunghezza massima dei percorsi minimi che studiamo è *|V| - 1* questo perchè è il numero dei vertici meno 1 (il vertice di partenza) non considerando i cicli

diciamo inoltre che l'albero dei cammini di peso minimo in G non è necessariamente unico, nel caso esistessero rami di peso uguale, l'albero dipenderebbe dalla sorgente di partenza

per determinare i cammini di di peso minimo viene utilizzata una tecnica chiamata **rilassamento**:
- il rilassamento viene effettuato *|V|-1* volte
- analizza il grafo nella sua interezza e aggiorna i pesi dei nodi sulla base dei percorsi minimi finora conosciuti

![[Pasted image 20240528114547.png]]
![[Pasted image 20240528114600.png]]
naturalmente è necessaria una procedura di inizializzazione all'inizio
il rilassamento opera controllando banalmente se esiste un nuovo percorso il cui peso è minore allora si aggiorna il peso precedente e si aggiorna il puntatore al predecessore

### Algoritmo di Bellman-Ford
>questo algoritmo funziona sia con pesi negativi sia con cicli negativi (nel caso in cui ve ne fossero restituirebbe false) e non è di tipo greedy

![[Pasted image 20240528115512.png]]
la proceduta si può concettualmente dividere in due parti
prima parte:
- data una sorgente in input viene inizializzato il grafo
- si esegue un ciclo for con *|V|-1* iterazioni 
- per ogni ramo viene chiamata la procedura di rilassamento che qualora fosse necessario aggiornerebbe i pesi del percorso minimo

seconda parte:
- vi è un nuovo ciclo che si esegue su tutti i rami
- se una volta eseguito il rilassamento (quindi concettualmente abbiamo trovato il percorso minimo) e ancora esistono percorsi con pesi minori allora siamo certamente in un ciclo negativo (perchè il valore diminuirebbe all'infinito per ogni iterazione)
- quindi restituiamo false

![[Pasted image 20240528120119.png]]

#### Correttezza e Complessità
Correttezza:
ad ogni iterazione *i-esima* si rilassano tutti gli archi, tra questi si rilasserà anche l'arco che fa parte del percorso minimo

Complessità:
nel caso dell'algoritmo di Bellman-Ford la complessità è data dalla moltiplicazione della complessità dei cicli che si iterano su tutti i vertici *|V|* e tutti gli archi *|E|* ottenendo quindi $$Θ(|V| · |E|)$$
### Bellman-Ford con Ordinamento Topologico
quando possiamo assumere che il grafo G sia aciclico allora un modo di ridurne la complessità è quella di ordinare il grafo trovando i pesi minimi dalla sorgente *s*, anche in presenza di pesi negativi

![[Pasted image 20240528120930.png]]
la procedura opera come segue:
- viene ordinato topologicamente il grafo
- viene inizializzato il grafo
- per ogni vertice ordinato si guardano gli adiacenti e si effettua un rilassamento

#### Correttezza e Complessità
Correttezza:
l'invariante in questo caso rimane il medesimo

Complessità:
tuttavia la complessità cambia perchè i cicli eseguiti sono diversi e otteniamo quindi $$Θ(|V| + |E|)$$

### Algoritmo di Dijkstra
le ipotesi sotto cui funziona questo algoritmo è quello di poter ammettere cicli ma non pesi negativi (positivi o zero)

l'algoritmo si appoggia alla struttura dati code di priorità in maniera molto simile all'algoritmo di Prim
![[Pasted image 20240528124303.png]]
la seguente procedura funziona nel seguente modo:
- si inizializza il grafo
- si inseriscono nella coda di priorità i nodi ordinati in base al peso
- fintanto che la coda non è vuota si estrae il minimo
- aggiungiamo il nodo corrente al sottoinsieme di rami che stiamo analizzando 
- il for controlla i nodi adiacenti al corrente ed esegue il rilassamento

 in questo caso Relax() nasconde un operazione di DecreaseKey per la coda 

#### Correttezza e Complessità
Correttezza:
l'invariante di questo algoritmo è che all'inizio di ogni iterazione del ciclo while *v.d = δ(s, v)* per ogni v ∈ S, ovvero è sempre vero che i vertici di ogni sottoinsieme S fino ad ora analizzati formano certamente il percorso più breve

Complessità:
anche qui come per l'algoritmo di Prim la complessità dipende da con quale tecnica e strategia vogliamo implementare la coda e che tipo di grafo abbiamo
Analizziamo il caso con grafo sparso e code di priorità basate su heap binarie avremo che:
- costruzione ed inizializzazione costano uguali perchè dipendono dai vertici quindi $$Θ|V|$$
- le estrazioni del minimo risultano essere $$Θ(|V| * log(|V|))$$
- mentre per i decrementi e di conseguenza come complessità totale (perchè la maggiore) abbiamo $$Θ(|E| * log(|V|))$$