---
date: '2025-03-09T17:26:55+01:00'
title: 'Cvičenie 04'
type: 'exercise'
weight: 4
---

V tomto cvičení si prejdeme rôzne implementácie stromu a grafu a práce s nimi. Práce so stromovými štruktúrami patria
medzi základné znalosti pre každého programátora a stretneme sa s nimi všade.

Cvičenie preverí vašu znalosť tried a pointrov, nakoľko prepojenia v rámci stromu, čo grafu musia byť presne.

{{< pdf "./pevs-dsa-graphs_trees.pdf" >}}

### Náplň

- implementáciu N-árneho stromu
- prechádzanie do hĺbky
- implementácia prefixové stromu a efektívne indexovanie stringov
- implementácia grafu

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
{{< card link="./task-4-1" title="Úloha 4.1" subtitle="Prechádzanie stromu do hĺbky" icon="document" >}}
{{< card link="./task-4-2" title="Úloha 4.2" subtitle="Prefixový strom (Trie)" icon="document" >}}
{{< card link="./task-4-2" title="Úloha 4.3" subtitle="Ováhovaný graf" icon="document" >}}
{{< /cards >}}
