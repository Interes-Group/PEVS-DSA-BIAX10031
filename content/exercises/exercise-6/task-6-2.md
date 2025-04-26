---
date: '2025-04-26T18:38:01+02:00'
title: 'Úloha 6.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte grafovú dátovú štruktúru, ktorá predstavuje neorientovaný ováhovaný graf, ktorého vrcholy sú
reprezentované celými číslami. Graf implementujte ako kolekciu zreťazených zoznamov dvojíc,
kde index kolekcie je číslo vrchola a v rámci zreťazeného zoznamu sú dvojice, kde prvý prvok predstavuje hranu grafu k
druhému vrcholu a druhý prvok dvoje predstavuje váhu hrany. Východzia hodnota váhy hrany je 1.

Graf na diagrame:

```text
(0)-----(1)
 |      /
 |    /
 |  /
 (2)
  |
  |
 (3)
```

Je reprezentovaný nasledovnou štruktúrou:

```text
0: [{1,1}, {2,1}]
1: [{0,1}, {2,1}]
2: [{0,1}, {1,1}, {3,1}]
3: [{2,1}]
```

### Príklady vstupov / výstupov programu

Pre demonštráciu funkčnosti kódu vypíšte vrcholy a ich hrany ako je uvedené v príklade vyššie.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
