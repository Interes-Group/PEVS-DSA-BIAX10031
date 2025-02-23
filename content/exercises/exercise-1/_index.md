---
date: '2025-02-13T18:17:00+01:00'

title: 'Cvičenie 01'
weight: 1
---

Toto cvičenie je zamerané na precvičenie základných konceptov programovania v jazyku C++. Budete pracovať s rôznymi
dátovými typmi, štandardným vstupom a výstupom, dynamickou alokáciou pamäte, poľami, reťazcami,
konštantami a vlastnými funkciami. Cieľom je posilniť ich pochopenie týchto konceptov a pripraviť ich na riešenie
zložitejších problémov.

{{< pdf "./pevs-dsa-uvod.pdf" >}}

### Náplň

- štandardný vstup a výstup
- práca s dynamicky alokovanými poliami
- práca s dátovým typom string

> [!IMPORTANT]
> Ak používate ako vývojové prostredie lokálny a editor a následnú kompiláciu cez terminál. Použite príkaz:
> ```shell
> g++ -o program -Wall -Wextra main.cpp
> ```

Pre vypracovanie týchto úloh odporúčam mať funkčné lokálne vývojové prostredie (VS Code, CLion a pod.) a kompilátor
jazyka C++.

> [!IMPORTANT]
> Nezabudnite každú alokovanú pamäť uvoľniť volaním operátorom `delete <premenná>`! Je dôležité si po sebe vždy
> upratať.

Riešenia na jednotlivé úlohy budú uverejnené neskôr.

## Úlohy

{{< cards cols="2" >}}
    {{< card link="./task-1-1" title="Úloha 1.1" subtitle="Súčet a priemer" icon="document" >}}
    {{< card link="./task-1-2" title="Úloha 1.2" subtitle="Párne čísla" icon="document" >}}
    {{< card link="./task-1-3" title="Úloha 1.3" subtitle="Transponovanie matice" icon="document" >}}
    {{< card link="./task-1-4" title="Úloha 1.4" subtitle="Palindrom" icon="document" >}}
    {{< card link="./task-1-5" title="Úloha 1.5" subtitle="Anagram" icon="document" >}}
    {{< card link="./task-1-6" title="Úloha 1.6" subtitle="Najdlhšia neklesajúca postupnosť" icon="document" tag="Bonus" tagType="warning" >}}
{{< /cards >}}
