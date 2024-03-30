# **Introduction au Projet de Langage de Programmation**

Ce projet vise à mettre en pratique les concepts étudiés dans le cadre du cours de techniques de compilation, en se concentrant particulièrement sur les différentes phases d'analyse (lexical, syntaxique, sémantique), ainsi que sur la génération de code cible et l'interprétation.

## **Grammaire Hors Contexte de notre langage**

1. **Programme** 
```
Prog = ‘begin’ { instructions } ‘end’ 
```

2. **Expression Simple** 
```
SExpr = Ident | Entier | Reel 
```

3. **Expression Arithmétique** 
```
ExprA = SExpr { opA SExpr } 

opA = (‘+’ | ‘-‘ | ‘\*’ | ‘/ ‘) 
```

4. **Déclaration de Variable** 
```
DeclVar = type ident [ ‘=’ ExprA { ‘,’ ident [ ‘=’ ExprA] } ] ‘;’ 
```

5. **Déclaration de Constante** 
```
DeclCste = ‘const’ type ident  ‘=’ ExprA { ‘,’ ident ‘=’ ExprA} ‘;’ 
```

6. **Affectation** 
```
Affectation = ident ‘=’ ExprA ‘;’ 
```

7. **Structure Alternative**
```
InstrIf = ‘if’ ‘(‘ condition ‘)’ ‘{‘ {instructions} ‘}’ [ partElse ] 

partElse = ‘else’ InstrIf | ‘{‘ {instructions} ‘}’  
```

8. **Structure Répétitive While** 
```
InstrWhile = ‘while’ ‘(‘ condition ‘)’ ‘{‘ {instructions} ‘}’ 
```

9. **Condition** 
```
Condition = ExprA opC ExprA [ opL condition ] 

opC = ( ‘<’ | ‘>’ | ‘<=’ | ‘>=’ | ‘==’ | ‘ !=’) 

opL = (‘&&’ | ‘||’) 
```

10. **Instruction de Lecture** 
```
InstrRead = scan ‘(‘ ident ‘)’ ‘;’ 
```

11. **Instruction d’Écriture** 
```
InstrWrite = print ‘(‘ ident ‘)’ ‘;’ 
```

12. **Instruction** 
```
Instr = DeclVar | DeclConst | Affectation | InstrWhile | InstrIf | InstrWrite | InstrRead | Cmt 
```

## **Contraintes Sémantiques du Langage**

13. **Sur les Variables**
- Deux variables ne peuvent pas avoir le même identifiant, idem pour les constantes.
- Une variable non déclarer ou non initialiser ne peut pas apparaitre dans une expression (arithmétique ou booléenne).''

14. **Sur les Contraintes**
- Une constante ne peut être déclarée qu'une seule fois et doit être initialisée lors de sa déclaration.
- On ne peut pas affecter une valeur à une constante après sa déclaration.

## **Unités Lexicales**

Les caractères spéciaux simples : `+, -, /, *, %, <, >, =, {, }, (, ), , “, ‘, [, ], !`

Les caractères spéciaux doubles : `==, ++, --, <=, >=, +=, -=, *=, /=, &&, ||, !=, //, /*, */`

Les mots-clés : `if, while, elseIf, const, “;”, for, do, int, double, float, string, Boolean, else`

Les constantes littérales : `145, -78  (en général tous les nombres)`, `‘a’, ‘b’ (en général toutes les lettres.)`

Les identificateurs : `i, j, nom_de_variable`

Tables de correspondances de nos lexèmes


|+ |add |
| :- | :- |
|= |affect |
|- |sub |
|/ |div |
|% |mod |
|\* |mul |
|< |inf |
|> |sup |
|{ |acoladOuv |
|} |acoladFerm |
|[ |crocheOuv |
|] |crocheFerm |
|! |not |
|‘ |apostrof |
|>= |supEgal |
|<= |infEgal |
|== |egal |
|++ |inc |
|-- |dec |
|// |cmtLigne |
|&& |and |
||| |or |
|!= |diff |
|“ |griffe |
|if |si |
|else |sinon |
|elseIf |sinonSi |
|while |tantQue |
|const |constante |
|nom\_de\_variable |ident |
|; |Line End |
|145 |entier |
|-75 |entier |
|‘b’ |lettre |
|2,5 |reel |
|/\* |debCmt |
|\*/ |finCmt |

Grammaire de chaque unité lexical (basé sur celle du java) : 
- lettre -> A | B ……. | Z | a | b ……. | z 
- chiffre -> 0 | 1 | 2 …… | 9 
- ident -> lettre {[‘\_’] lettre | chiffre} 
- nombre -> [signe]chiffre{chiffre} 
- réel -> [ signe ] chiffre { chiffre } [ ‘,’ Chiffre { chiffre } ]
