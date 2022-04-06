# Lambda calcolo 

## Definzioni base
Introdotto da Alonzo Church nel 1930 come modello di computabilità.

E' un sistema per la manipolazione di espressioni, dove un espressione può essere un:
  - nome
  - funzione
  - applicazione di funzione (specializzazione di una funzione con valori)

Scritto nella sintassi di Backus-Naur:

  **\<espressione\>::= \<nome\>|\<funzione\>|\<applicazione\>**
  
Una funzione è un'astrazione di una λ-espressione con nome:

  **\<funzione\>::= λ\<nome\>.\<espressione\>**
  
**nome** rappresenta la variabile bound (l'equivalente di un parametro in una funzione c#)

**.** fa da separatore tra nome e l'espressione che rappresenta il corpo della funzione

Un'applicazione è una specializzazione di una funzione a cui assegnamo un valore alla variabile bound:

  **\<applicazione\>::=(\<espressionedifunzione\>\<espressionediargomento\>)**
 
dove:
  
  **\<espressionedifunzione\>::= \<espressione\>**
  
  **\<espressionediargomento\>::= \<espressione\>**
  
L'**espressionedifunzione** contiene la funzione che verrà specializzata con il valore di **espressionediargomento**

Un'applicazione di funzione si chiama anche **bound pair** e si dice che si applica l'**espressionedifunzione** all'**espressionediargomento**

Per valutare si possono seguire due diversi ordini di valutazione:
  - ordine applicativo: si valutano le espressioni a partire da quella più annidata
  - ordine normale: si valutano le espressioni a partire da quella più a sinistra

(vedi https://courses.cs.washington.edu/courses/cse505/99au/functional/applicative-normal.pdf)



