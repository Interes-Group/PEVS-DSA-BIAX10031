---
date: '2025-03-23T20:54:42+01:00'
title: 'Cvičenie 05'
type: 'exercise'
weight: 5
---

Toto cvičenie si precvičíme triediace algoritmy, bubble sort, merge sort a quick sort na vektore a liste.
Pre programátora je dôležité vedieť ako správne a efektívne zotriediť prvky v štruktúre či kontajnery.

{{< pdf "./pevs-dsa-sort.pdf" >}}

### Náplň

- Implementácia Bubble sort na kontajnery `vector<string>`
- Implementácia Merge sort na kontajnery `list<string>`
- Implementácia Quick sort na kontajnery `list<string>`

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
    {{< card link="./task-5-1" title="Úloha 5.1" subtitle="Bubble sort" icon="document" >}}
    {{< card link="./task-5-2" title="Úloha 5.2" subtitle="Merge sort" icon="document" >}}
    {{< card link="./task-5-3" title="Úloha 5.3" subtitle="Quick sort" icon="document" >}}
{{< /cards >}}
