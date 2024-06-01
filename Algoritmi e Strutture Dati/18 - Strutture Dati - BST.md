| Lezione Precedente | [[17 - Strutture Dati - Alberi]] |
| ------------------ | -------------------------------- |
| Lezione Successiva | [[19 - Strutture Dati - RBT]]    |
_gli appunti fanno riferimento alla lezione 20 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Alberi binari di ricerca (BST)
>I BST sono un caso di albero particolare con le seguenti caratteristiche
>- struttura *dinamica*
>- basata sull'*ordinamento*
>- implementata in maniera *sparsa* 
>
>Inoltre i BST permettono operazioni come
>- inserimento
>- cancellazione
>- ricerca
>- minimo
>- massimo
>- successore
>- precedente

diciamo inoltre che i BST devono rispettare le seguenti due proprietà:
- i nodi di un sottoalbero sinistro devono avere valore minore del padre *y.key < x.key*
- i nodi di un sottoalbero destro devono avere valore maggiore del padre *x.key > y.key*
![[Pasted image 20240525170930.png]]
*esempio di BST*

>il risultato di una visita **in order** in un BST restituisce le chiavi ordinate, nel caso di sopra le chiavi avrebbero ordine {3, 5, 7, 8, 10}
 
### Operazioni e Complessità
nel caso di *creazione* i BST si creano vuoti come qualsiasi albero perciò non ci soffermiamo su questa operazione

#### Ricerca generica
![[Pasted image 20240525171103.png]]
Sapendo della proprietà intrinseca dei BST di avere a sinistra gli elementi più piccoli e a destra gli elementi più grandi di ogni sottoalbero possiamo facilmente capire come funziona l'operazione

Complessità:
la procedura è tail-ricorsiva, nel **caso peggiore** sappiamo che bisogna scorrere tutti gli elementi per trovare quello desiderato, quindi la complessità sarà $$Θ(n)$$
nel **caso medio** invece l'albero ha altezza logaritmica e quindi ha complessità $$Θ(log(n))$$
#### Ricerca Minimo e Massimo
![[Pasted image 20240525171123.png]]
ancora una volta facendo riferimento alla proprietà fondamentale dei BST sappiamo per certo che l'elemento minore sarà l'ultima foglia di tutti i rami sinistri, e viceversa nel caso in cui volessimo ricercare il massimo

Complessità:
nel **caso peggiore** la complessità equivale a quella delle liste, perchè potremmo avere tutti i rami in fila quindi dover percorrere tutti gli elementi per trovare minimo/massimo $$Θ(n)$$
e nel **caso medio** invece sempre per la natura di altezza logaritmica avremo $$Θ(log(n))$$

#### Successore e Predecessore
![[Pasted image 20240525171153.png]]
in questo caso la soluzione non è banale perchè, l'elemento dal valore sccessivo o precedente non è detto che sia l'elemento collegato, vedi esempio sotto:
![[Pasted image 20240525171213.png]]
la soluzione quindi: 
- se il nodo del quale vogliamo ricercare il successore (in questo caso) ha un figlio destro allora sarà certamente il successore
- altrimenti ripercorriamo il ramo fintanto che ci sono elementi per cercare il successore

Complessità:
anche in questo caso la situazione rimane la medesima quindi per il **caso peggiore** abbiamo $$Θ(n)$$ mentre per il **caso medio** abbiamo $$Θ(log(n))$$

#### Inserimento
![[Pasted image 20240525171240.png]]
nel caso dell'inserimento dobbiamo comprendere anche la ricerca in modo da capire dove posizionare l'elemento, in **tutti** i casi comunque il nuovo elemento verrà inserito attaccato ad una foglia senza figli con il corretto puntatore, che inizialmente è impostato a *nil*

*l'invariante del ciclo é: la posizione corretta di z é nel sottoalbero radicato in x, e y ne mantiene il padre.*

Complessità:
allo stesso modo e come vedremo per tutte le operazioni di BST anche in questo caso abbiamo che nel **caso peggiore** la complessità è $$Θ(n)$$ mentre nel caso medio abbiamo $$Θ(log(n))$$

#### Eliminazione
![[Pasted image 20240525171309.png]]
in questo caso la situazione si complica parecchio, abbiamo due procedure, Delete (che in realtà non elimina niente) e Trapianto che gestisce ed aggiorna i puntatori isolando l'elemento da eliminare

Distinguiamo tre casi di difficoltà crescente
- 1) *z* (l'elemento da eliminare) non ha figli, in questo caso si eliminano semplicemente i puntatori
- 2.a) *z* ha solo un figlio destro, si collega il figlio al padre
- 2.b) *z* ha solo un figlio sinistro, same
- 3) *z* ha due figli, il caso più complicato, occorre trovare il successore o il minimo, andare ad isolarlo, e poi collegare i puntatori agli elementi vicini 

Complessità: 
allo stesso modo e come vedremo per tutte le operazioni di BST anche in questo caso abbiamo che nel **caso peggiore** la complessità è $$Θ(n)$$ mentre nel caso medio abbiamo $$Θ(log(n))$$

#### Confronto
![[Pasted image 20240525171335.png]]

