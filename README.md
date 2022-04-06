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


## funzione identity

è una funzione che si limita a restituire il parametro usato.

λx.x

proviamo a passare come parametro alle funzione identity la funzione identity stessa

(λx.x λx.x) => λx.x

## funzione self-application

è una funzione che applica il suo argomento al suo argomento.

λs.(s s)

Proviamo ad applicare la funzione identity a self-application:

(λx.x λs.(s s)) => λs.(s s)

Proviamo ora ad applicare la funzione self-application alla funzione identity:

(λs.(s s) λx.x) => (λx.x λx.x) => λx.x

Proviamo ora ad applicare la funzione self-application a se stessa:

(λs.(s s) λs.(s s)) => (λs.(s s) λs.(s s))

Applicando self-application a se stessa otteniamo nuovamente l'applicazione di self-application a se stessa in un loop infinito. 

Da questo si può capire non tutte le valutazioni di espressioni terminano; inoltre **non esiste un modo di sapere se la valutazione di un'espressione termini o no**.
