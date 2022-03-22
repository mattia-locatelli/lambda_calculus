# Lambda calcolo 

## Definzioni base
Introdotto da Alonzo Church nel 1930 come modello di computabilità.

E' un sistema per la manipolazione di espressioni, dove un espressione può essere un:
  - nome
  - funzione
  - applicazione di funzione (specializzazione di una funzione con valori)

Scritto nella sintassi di Backus-Naur:

  **\<espressione>::= \<nome>|\<funzione>|\<applicazione>**
  
Una funzione è un'astrazione di una λ-espressione con nome:

  **\<funzione>::= λ\<nome>.\<espressione>**
  
in questo caso nome rappresenta la variabile bound (l'equivalente di un parametro in una funzione c#)
