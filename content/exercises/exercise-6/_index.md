---
date: '2025-04-26T18:25:20+02:00'
title: 'Cvičenie 06'
type: 'exercise'
weight: 6
---

Toto cvičenie je zamerané na grafy a algoritmy, ktoré sa často používajú s použitím takejto dátovej štruktúre.
Pre programátora je dôležité pochopiť aké prehľadávania a optimalizácie je možné vykonať nad grafovou štruktúrou,
nakoľko modelujú komplexné vzťahy medzi entitami.

{{< pdf "./pevs-dsa-graphs.pdf" >}}

### Náplň

- implementácia jednoduchého grafu pomocou matice
- implementácia jednoduchého grafu pomocou zreťazených polí
- implementácia Dijkstrovho algoritmu najkratšej cesty

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
    {{< card link="./task-6-1" title="Úloha 6.1" subtitle="Graph ako matica" icon="document" >}}
    {{< card link="./task-6-2" title="Úloha 6.2" subtitle="Graph ako zreťazené zoznamy" icon="document" >}}
    {{< card link="./task-6-3" title="Úloha 6.3" subtitle="Dijkstra algoritmus" icon="document" tag="Komplexné" tagType="info" >}}
{{< /cards >}}
