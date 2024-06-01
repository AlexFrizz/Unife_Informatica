| Lezione Precedente | [[16 - Strutture Dati - Insiemi Disgiunti]] |
| ------------------ | ------------------------------------------- |
| Lezione Successiva | [[18 - Strutture Dati - BST]]               |
_gli appunti fanno riferimento alla lezione 19 del corso 20/21 le slide invece sono 'Correttezza, Complessità e notazione asintotica' del classroom 22/23_

## Introduzione agli Alberi
>gli alberi sono strutture dati che generalizzano le liste, e con le seguenti caratteristiche
>- struttura *dinamica*
>- memorizzazione *sparsa*
>- *basate/non basate su ordinamento*

Un albero radicato (semplicemente albero) è un grafo aciclico connesso tale che ogni nodo è connesso da al più un cammino, un albero è *k-ario* se ogni nodo ha al più *k* figli distinti

**Topologia degli alberi:**
ogni albero può avere diverse strutture a seconda di come sono posizionati i nodi e i figli (facciamo riferimento solo ad alberi binari)
- *completo*: ogni livello è completo, cioè tutti i nodi di uno stesso livello hanno esattamente zero o due figli
- *quasi completo:* ogni livello tranne eventualmente l'ultimo è completo
- *bilanciato*: tra il percorso semplice più breve e quello semplice più lungo la radice ed una foglia esiste una differenza al massimo costante
- *pieno*: ogni nodo ha zero o due figli 


un albero può anche essere *diretto* e *indiretto*
- *diretto:* tale che ogni sottoalbero ha un nodo predeterminato chiamato radice 
- *indiretto:* tale che la radice viene individuata in maniera arbitraria così come la direzionalità dei cammini

### Altezza
 aggiungiamo inoltre qualche definizione aggiuntiva 
 - *foglie:* nodi senza figli
 - *altezza:* l'altezza di un albero è il numero massimo di archi su un percorso semplice dalla radice ad una foglia

quando un albero non ha nessuna delle proprietà topologiche menzionate sopra allora la sua altezza è 
- nel caso peggiore: *Θ(n)* 
- ne caso medio: *Θ(log(n))*


### Struttura
analizzando un albero binario diciamo che possiede almeno:
- una chiave (x.key)
- puntatore al padre (x.p)
- puntatore al figlio destro (x.right)
- puntatore al figlio sinistro (x.left)

tutti i puntatori sono *nil* quando non sono definiti
![[Pasted image 20240525165225.png]]

Esistono tre modi principali per visionare gli elementi e la struttura di un albero, si chiamano *visite* e possono essere:
- **in order** 
- **pre order**
- **post order**

#### In Order
![[Pasted image 20240525165824.png]]
particolarmente utile per i BST perchè restituisce i valori delle chiavi ordinate, la visita avviene nel seguente ordine:
- si visita sottoalbero sinistro
- si visita il nodo corrente
- si visita il sottoalbero destro

#### Pre Order
![[Pasted image 20240525170228.png]]
tipicamente usata per operazioni di copia dell'albero, la visita avviene nel seguente ordine:
- si visita il nodo corrente
- si visita sottoalbero sinistro
- si visita il sottoalbero destro

#### Post Order
![[Pasted image 20240525170048.png]]
tipicamente usata per operazioni di cancellazione perchè non lascia nodi pendenti, la visita avviene nel seguente ordine:
- si visita sottoalbero sinistro
- si visita il sottoalbero destro
- si visita il nodo corrente

vediamo un esempio prendendo in esame i risultati delle visite dell'albero nell'immagine superiore
- in order: {7, 5, 8, 1, 4}
- pre order: {5, 7, 1, 8, 4}
- post order: {7, 8, 4, 1, 5}

