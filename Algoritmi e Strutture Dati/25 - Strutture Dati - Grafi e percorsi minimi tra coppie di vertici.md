| Lezione Precedente | [[24 - Strutture Dati - Grafi e percorsi minimi]] |
| ------------------ | ------------------------------------------------- |

_gli appunti fanno riferimento alla lezione 28 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Percorsi minimi tra tutte le coppie di vertici
in questa lezione analizziamo il problema servendoci della tecnica chiamata *programmazione dinamica*, l'obbiettivo è quello di dividere il problema in *sotto-problemi comuni* che abbiano appunto un legame tra di loro

Vogliamo quindi trovare il percorso minimo tra tutte le coppie di vertici, per farlo utilizzeremo necessariamente dei grafi basati su matrici di adiacenza

### Algoritmo di Floyd-Warshall
l'algoritmo in questione serve proprio per trovare una soluzione al nostro problema, funziona anche con pesi negativi (non cicli)

l'idea di base è la seguente: dati due vertici *i* e *j* qualsiasi e considerato il percorso minimo tra i due, controlliamo tutti i vertici diversi da *i* e *j* che compaiono in questo percorso e che vengono chiamati *intermediari* e vediamo se esiste un percorso migliore

![[Pasted image 20240529094705.png]]
l'algoritmo funziona come segue:
- viene inizializzata la matrice di adiacenza *W*
- si esegue un ciclo for per ogni vertice del grafo
	- ad ogni iterazione di *k* si genera una nuova matrice *D^k*
	- si scorre poi tutta la matrice con i due for su *i* e *j* 
		- si trova il percorso minimo considerando gli intermediari


#### Correttezza e Complessità
Correttezza:
la correttezza dell'algoritmo è basata sulle osservazioni precedenti

Complessità:
la complessità invece è data dal costo dei tre cicli for presenti nell'algoritmo, tutti hanno *n* iterazioni quindi il costo sarà $$Θ(|V|^3)$$

