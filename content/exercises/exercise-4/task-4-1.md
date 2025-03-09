---
date: '2025-03-09T17:27:51+01:00'
title: 'Úloha 4.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte **N-árny strom**, t.j. strom ktorého uzol môže mať 0 až N detí. Každý uzol stromu má označenie '**label**'
typu `string`. **Deti** uzla musia byť **unikátne**. Uzol implementujte ako triedu. Aplikujte princípy zaprúzdrenia (private
atribúty, getter, setter a ďalšie).
Pre uzol implementujte metódu:

- `bool add_child(Node*)` - Pridá do uzla nový detský uzol. Metóda **pridá uzol ako dieťa** len ak poskytnutý uzol **ešte nie je
  medzi detskými** uzlami. Metóda vráti true ak bol uzol úspešne pridaný medzi deti, inak vráti false.

Implementujte metódu `string &traverse(Node*)`, ktorá bude implementovať **prechádzanie stromu do hĺbky**. Metódu
implementujte pomocou **rekurzie**. Metóda vráti `string`, ktorý obsahuje označenie uzlov, cez ktoré algoritmus prechádzal
oddelené čiarkou.

Pre demonštráciu vypracovania vytvorte strom aspoň hĺbky 3 s ľubovolným počtom uzlov. Následne zavolajte metódu `traverse()`
a výstup metódy vypíšte na štandardný výstup (`cout`).

### Príklady vstupov / výstupov programu

Majme strom:

![strom](/images/task41-tree.png)

Ak je strom implementovaný podľa zadania, tak výstup metódy traverse je 

```
1, 2, 5, 6, 7, 8, 4, 3
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
