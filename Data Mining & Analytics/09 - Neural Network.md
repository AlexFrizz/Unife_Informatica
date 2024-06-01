*gli appunti fanno riferimento alle slide della lezione 9 del classroom di data mining & analytics del 22/23*

| Lezione Precedente | [[08 - Linear Models]] |
| ------------------ | ---------------------- |
| Lezione successiva |                        |
## Bio-Inspired Multi-Layer Network
- abbiamo visto come una delle maggiori debolezze dei modelli lineari sia l'incapacità di imparare arbitrariamente decision boundary
- al contrario invece di algoritmi come k-NN o decision trees

- un approccio per superare questo ostacolo è costruire reti più complesse unendo più percettroni
![[Pasted image 20240321095805.png]]
*l'immagin mostra una two-layer network*

*funzionamento*
- si calcolano le attivazioni dei nodi (hidden unit) basandosi sugli input e i pesi degli input
- poi si calcola l'attivazione dell'output data l'attivazione delle hidden unit e dal secondo layer di pesi
- la differenza più grande è che le unità nascoste calcolano una funzione non lineare dei loro input
- questa si chiama funzione di attivazione o link function
- se *wi,d* sono i pesi che connettono gli input *d* alle unità nascoste *i* allora l'attivazione dell'unità nascosta *i* sarà: $$hi = f(wi * x)$$

- un esempio di link function è la funzione segno
- se il segnale è negativo l'attivazione sarà *-1* altrimenti *+1*

*problema*
- non è differenziabile
- un approssimazione popolare è la funzione tangente iperbolica, *tanh*
![[Pasted image 20240321101126.png]]
- in questo modo, facendo questa approssimazione la funzione segno diventa differenziabile
- per via della sua forma ad S questa funzione viene chiamata anche *funzione Sigmoide*

- assumendo di utilizzare la funzione *tanh* come attivazione abbiamo
![[Pasted image 20240321101315.png]]
*rete a due starti dove la funzione segno f() è approssimata utilizzando tanh*

