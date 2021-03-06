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


## funzione identity ( Idiot - I )

è una funzione che si limita a restituire il parametro usato.

λx.x

proviamo a passare come parametro alle funzione identity la funzione identity stessa

(λx.x λx.x) => λx.x

## funzione self-apply ( Mockingbird - M )

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

Possiamo usare la funzione apply per creare nuove versioni di una funzione. Per esempio possiamo definire una funzione identity2 come:

def identity2 = λ.x((apply identity) x)

Applichiamo ora identity2 a identity:

(identity2 identity) =>

(λ.x((apply identity) x) identity) =>

((apply identity) identity) =>

((λfunc.λarg(func arg) identity) identity) =>

λarg((identity arg) identity => 

(identity identity) =>

identity

Dimostriamo ora che identity e identity2 sono equivalenti. Applichiamo identity2 ad un argomento generico <argument>
  
(identity2 argument) =>
  
((λ.x(apply identity)x) \<argument\>) =>

((apply identity) \<argument\>) =>

(((λ.funcλ.arg(func arg)) identity) \<argument\> =>

(identity \<argument\>) =>

λx.x(\<argument\>) =>

\<argument\>

Da ciò possiamo ricavare che:
  
(apply \<function\>) =>

(λ.funcλ.arg(func arg) \<function\>)

λ.arg(<function> arg)
  
sostiuiamo ora una argomento generico \<argument\> alla funzioneche abbiamo definito:
  
(λ.arg(<function> arg) \<argument\>) =>

(\<function\> \<argument\>)
  
otteniamo \<function\> \<argument\> che è l'applicazione di \<function\> ad \<argument\> ed equivale ad utilizzare la funzione apply.  
  
In generale invocare apply su una funzione \<function\> con argomenti generici \<argument\> equivale a invocare \<function\> su  \<argument\>

apply \<function\> \<argument\> =>

(((λ.funcλ.arg (func arg)) \<function\>) \<argument\>) =>

(λ.arg(\<function\> arg) \<argument\>) =>

\<function\> \<argument\>  
 
## Selezione argomenti e funzioni di pairing degli argomenti

### Selezione del primo tra due argomenti ( Kestrel - K )

Definiamo la funzione:
  
def select_fisrt = λfirst.λsecond.first  

questa funzione restituisce sempre il primo dei due argomenti. Vediamo un esempio cond due argomenti generici:

select_first \<arg1\> \<arg2\>

(((λfirst.λsecond.first) \<arg1\>) \<arg2\>) =>

((λsecond.\<arg1\>) \<arg2\>) =>

\<arg2\>  
  
### Selezione del secondo tra due argomenti ( Kite - KI )
  
 Definiamo la funzione:
  
 def select_second = λfirst.λsecond.second
  
 questa funzione restituisce sempre il secondo argomento. Vediamo un esempio con due argomenti generici
 
 select_second \<arg1\> \<arg2\> =>
  
 (((λfirst.λsecond.second) \<arg1\>) \<arg2\>) =>
 
 (λsecond.second) \<arg2\> =>
 
 \<arg2\>
   
## Accoppiare due argomenti ( Vireo - V )

Definiamo la funzione:
  
def make_pair = λfirst.λsecond.func((func fisrt) second)
  
proviamo ora ad applicare la nuova funzione a due delle funzioni che abbiamo già incontrato (apply e identity):

((make_pair identity) apply) =>

((λfirst.λsecond.λfunc(funct first) second) identity ) apply) =>

((λsecond.λfunc(func idenity) second ) apply) =>
  
(λfunc(func identy) apply )

Se applichiamo la funzione appena trovata a select_first otteniamo:
  
((λfunc(func identy) apply ) select_first) =>

((select_first identity) apply ) =>

((λa.λb(a) identity ) apply ) =>

(λb(identity) apply) =>

identity  

Se invece la applichiamo a select_second otteniamo:
  
((λfunc(func identy) apply ) select_second) =>

((apply identity) select_second ) =>

((λfunc.λarg(func arg)) identity) select_second ) =>
 
((λarg(identity arg)) select_second ) =>
  
identity select_second =>

λx.x select_second => 
 
select_second
  
In genere se applichiamo make_pair a due argomenti generici <arg1> e <arg2> otteniamo:

make_pair \<arg1\> \<arg2\> =>
  
(((λfirst.λsecond.λfunc ((func first) second)) \<arg1\>) \<arg2\>) =>
  
(((λsecond.λfunc ((func \<arg1\>) second)) \<arg2\>) =>
  
λfunc((func \<arg1\>) \<arg2\>)
  
## Variabili libere e legate

è possibile avere lo stesso nome di variabile nella definizione di una funzione. Ad esempio:
  
(λf.(f λf.f) λs.(s s)) =>
  
((λs.(s s) λf.f)) =>
  
(λf.f λf.f) =>
  
λf.f
  
Nell'esempio precedente abbiamo sostituito λs.(s s) solo nella prima occorrenza di f nella funzione ( si dice che è in scope ). Solo questa infatti è la variabile bound nell'espressione precedente. Le occorrenze di f in λf.f sono un'altra variabile distinta dalla prima occorenza di f ( si dice che sono fuori scope ).
  
