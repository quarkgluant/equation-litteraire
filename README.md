# EQUATION LITTERAIRE  
  
Encore une fois, cet exercice est tiré de l'**excellent**  site [Exercism](https://exercism.io) où vous pouvez améliorer
vos compétences en Ruby (ou bien d'autres langages) grâce à la présence de Mentors qui vous conseillent pour améliorer 
votre code. Le kata originel se nomme alphametics.  

Le but: écrire une fonction qui résoud des équations (ou puzzles) alphamétiques ou cryptarithmes.  

les [Alphametics](https://en.wikipedia.org/wiki/Alphametics) ou [cryptarithmes](https://fr.wikipedia.org/wiki/Cryptarithme)
sont des équations (aussi nommées puzzles) où l'on va remplacer toutes les lettres par des chiffres  
 
La plus connue `SEND + MORE = MONEY`:

```text
  S E N D
  M O R E +
-----------
M O N E Y
```

ce qui donne , après remplacement:

```text
  9 5 6 7
  1 0 8 5 +
-----------
1 0 6 5 2
```

Pour être valide, chaque lettre doit être remplacé par un seul et même chiffre (ici, chaque `S` est un `9`, un S ne peut alors être 
un autre chiffre), et un chiffre ne doit pas être utilisé pour plusieurs lettres (ici, `9`, associé à `S`, ne peut être associé 
à une autre lettre), et, *bien-sûr*, l'addition doit être valide mathématiquement.  

Donc, chaque lettre représente un chiffre différent, et, dernière contrainte, le premier chiffre d'un nombre ne peut être
zéro, on ne peut pas avoir comme nombre `01234` mais `1234`  

Cet exercice est accessible quel que soit votre niveau, et si vous passez les tests jusqu'à 7 lettres, c'est gagné. Les 3
derniers tests, pour être réalisés dans un temps raisonnable, nécessite plusieurs optimisations. Pour info, sur mon laptop
(Linux, 8 cores, 32 Go RAM), ma première version passait les 7 premiers tests en 22 secondes, la dernière passe tous les tests
en 4 secondes

Pour les anglophiles:  
[Alphametics](https://en.wikipedia.org/wiki/Alphametics) is a puzzle where
letters in words are replaced with numbers.

For example `SEND + MORE = MONEY`:

```text
  S E N D
  M O R E +
-----------
M O N E Y
```

Replacing these with valid numbers gives:

```text
  9 5 6 7
  1 0 8 5 +
-----------
1 0 6 5 2
```

This is correct because every letter is replaced by a different number and the
words, translated into numbers, then make a valid sum.

Each letter must represent a different digit, and the leading digit of
a multi-digit number must not be zero.

Write a function to solve alphametics puzzles.

* * * *


