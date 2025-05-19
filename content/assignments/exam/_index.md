---
date: '2025-05-19T16:37:14+02:00'
title: 'Skúškové Zadanie'
---

# Anglicko-slovenský prekladový strom

Napíšte program v jazyku C alebo C++, ktorý bude **implementovať funkcionalitu anglicko-slovenského slovníka
prostredníctvom lexikografického stromu** (Trie), v ktorom budú anglické slová (písané malými písmenami od 'a' po 'z').

Vrcholy lexikografického stromu budú mať:

- Atribút pole 26 smerníkov (pointerov) na nasledovné vrcholy lexikografického stromu reprezentujúce jednotlivé písmená.
- Atribút `jeSlovo` (typu `int`), ktorá určuje, či vrchol predstavuje koniec slova.
- Atribút `preklad` (typu buď `string` alebo `vector` alebo pole znakov, t.j. `char[]`), ktorá bude uchovávať
  slovenský preklad daného slova v prípade, ak daný vrchol predstavuje koniec anglického slova.

Po spustení programu vykoná program v cykle:

### Voľba 1

Pri zadaní _"1"_ si program vypýta anglické slovo a jeho slovenský preklad a následne vloží anglické slovo do
lexikografického stromu a jeho slovenský preklad uloží do premennej preklad vrcholu, ktorý predstavuje posledné písmeno
vkladaného anglického slova.
**(10 bodov)**

Ak dané anglické slovo už v lexikografickom strome je, nahradí sa jeho pôvodný preklad vkladaným prekladom.
**(5 bodov)**

### Voľba 2

Pri zadaní _"2"_ si program vypýta anglické slovo a vyhľadá, či slovo je v slovníku (v lexikografickom strome). Ak áno,
potom vypíše jeho slovenský preklad. **(10 bodov)**

Ak nie, vypíše hlášku, že anglické slovo v slovníku nenašiel. **(5 bodov)**

### Voľba 3

Pri zadaní _"3"_ si program vypýta anglické slovo a ak sa nachádza v slovníku, potom ho vymaže, t.j. zmení hodnotu `jeSlovo`
vrcholu pre posledné písmeno anglického slova na **0**. **(5 bodov)**

V prípade, že vymazané anglické slovo nie je prefixom iného anglického slova v slovníku, vymaže tie vrcholy,
ktoré nie sú súčasťou žiadneho iného anglického slova uloženého v slovníku (v lexikografickom strome). **(10 bodov)**

### Voľba 4

Pri zadaní _"4"_ vypíše program celý slovník tak, že v jednotlivých riadkoch budú dvojice anglické slovo, pomlčka a
slovenský preklad anglického slova. **(15 bodov)**

Na začiatku bude slovník (lexikografický strom) prázdny.

Program okomentujte, zároveň ako komentár **na začiatku programu uveďte svoje meno a priezvisko.**

Zdrojový kód programu zašlite emailom s predmetom "DSA Skúška" na emailovú adresu prednášajúceho predmetu.
