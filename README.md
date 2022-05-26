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

## funzione self-apply

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

## Funzione apply

è una funzione che prende come argomento una funzione che è l'applicazione dell'argomento della prima funzione all'argomento della seconda funzione. 

λfunc.λarg.(func arg)

Proviamo ad applicare la funzione identity all' application-function:

(λfunc.λarg.(func arg) λx.x) λs.(s s) => (λarg.(λx.x arg) λs.(s s)) => (λx.x λs.(s s)) => λs.(s s)

## Sintassi aggiuntiva

per comodità definiamo della sintassi aggiuntiva (nel nostro caso si tratta di regole di sostituzione) che non fa parte delle notazioni del lambda calcolo ma ci rende più facile e veloce scrivere espressioni lambda.

Definiamo quindi la notazione:

def \<nomefunzione\> = \<funzione\>
  
che ci permette di assegnare un \<nomefunzione\> a una \<funzione\> e sostuirla all'interno di una applicazione di funzione:
  
(\<nomefunzione\> \<argomento\>) == (\<funzione\> \<argomento\>)

La sostituzione di una variabile bound nel corpo di una funzione si chiama **β-riduzione**:

(\<funzione\> \<argomento\>) => \<espressione\>
  
ad esempio:
  
def identity = λx.x

(identity u) => (λx.x u) => u
  
## Funzioni da funzioni

Possiamo usare la funzione apply per creare nuoveversioni di una funzione. Per esempio possiamo definire una funzione identity2 come:

def identity2 = λ.x((apply identity) x)

Applichiamo ora identity2 a identity:

(identity2 identity) =>

(λ.x((apply identity) x) identity) =>

((apply identity) identity) =>

((λfunc.λarg(func arg) identity) identity) =>

λarg((identity arg) identity => 

(identity identity) =>

identity


 
