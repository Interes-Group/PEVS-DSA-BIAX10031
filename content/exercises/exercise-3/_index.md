---
date: '2025-03-02T12:22:28+01:00'
title: 'Cvičenie 03'
type: 'exercise'
---

Na tomto cvičení sa zoznámime s implementáciou základných dynamických dátových štruktúr, ktoré sú kľúčové pre
efektívne spracovanie a organizáciu údajov. Cieľom je pochopiť vnútorné fungovanie **zreťazeného zoznamu, fronty a
zásobníka**, pričom ich budeme implementovať od základu bez použitia knižnicových kontajnerov. Dôraz je kladený
na správu dynamickej pamäte, efektívne pridávanie a odoberanie prvkov, ako aj na rozdiely medzi jednotlivými štruktúrami
a ich využitie v praxi.

{{< pdf "./pevs-dsa-stl-containers.pdf" >}}

### Náplň

- implementácie rôznych druhov zoznamom
- implementácia fronty a princípu FIFO
- implementácia zásobníka a princípu LIFO
- implementácia množiny

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
{{< card link="./task-3-1" title="Úloha 3.1" subtitle="Dopredu zreťazený zoznam" icon="document" >}}
{{< card link="./task-3-2" title="Úloha 3.2" subtitle="Obojstranne zreťazený zoznam" icon="document" >}}
{{< card link="./task-3-3" title="Úloha 3.3" subtitle="Cyklický zoznam" icon="document" >}}
{{< card link="./task-3-4" title="Úloha 3.4" subtitle="Zásobník (stack)" icon="document" >}}
{{< card link="./task-3-5" title="Úloha 3.5" subtitle="Fronta (queue)" icon="document" >}}
{{< card link="./task-3-6" title="Úloha 3.6" subtitle="Unikátna nezoradená množina" icon="document" >}}
{{< /cards >}}
