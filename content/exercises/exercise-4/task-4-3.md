---
date: '2025-03-09T17:27:55+01:00'
title: 'Úloha 4.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte graf, ktorého hrany majú definovanú váhu, t.j. **ováhovaný graf**. Uzol grafu má označenie '**label**' typu
`string`. Hrana spájajúca dva uzly musí mať definovanú **váhu** typu `int`. Hrany medzi uzlami nemajú orientáciu a tak 
prechádzanie medzi uzlami môže byť oboma smermi.

Pre graf implementujte nasledovné metódy:

- `bool add_vertex(Vertex*, int*)` - Pridanie nového uzla do grafu. Metóda má byť ako public člen triedy Vertex. Prvý
  argument je **pointer na uzol** s ktorým chceme prepojenie vytvoriť. Druhý argument je **váha hrany**, ktorá bude vytvorená
  medzi týmito dvomi uzlami.
- `int count(vector<string>*)` - Ak váha hrany predstavuje cenu, ktorú treba zaplatiť aby sme prešli po danej hrane na
  ďalší uzol, tak táto metóda vráti celkovú cenu cesty z uzla na uzol. Argument metódy je cesta prechádzania uzlov, kde
  prvok vektora je označenie (_label_) uzla. Ak taká cesta v grafe existuje metóda vráti jej celkovú cenu. Ak cesta
  neexistuje, t.j. neexistuje hrana, ktorá by prepojila dva uzly, ktoré sú za sebou definové vo vstupnom vektore, metóda
  vráti hodnotu `-1`.

### Príklady vstupov / výstupov programu

Ak máme graf s nasledujúcou štruktúrou:

![graph](/images/task43-weighted-graph.png)

Volanie funkcie pre výpočet celkovej ceny cesty (celkovej váhy) má nasledovné výsledky:

```cpp
count({1,4,3}) == 8;
count({2,5,1,4}) == 14;
count({3,1,2,5,1}) == 18;
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
