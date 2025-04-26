---
date: '2025-02-14T17:05:40+01:00'
title: 'Cvičenie 02'
weight: 2
---

Úlohy tohto cvičenia sa zameriavajú na precvičenie práce s triedami v jazyku C++ a ich vzájomné interakcie v rôznych
scenároch. Úlohy sú konštruované tak aby viedli k navrhovaniu tried, implementácii ich metód a atribútov, ako aj k
aplikácii objektovo-orientovaných princípov v praktických situáciách.

{{< pdf "./pevs-dsa-classes.pdf" >}}

### Náplň

- Definícia a implementácia tried
- Zapuzdrenie (encapsulation)
- Dynamická alokácia pamäte
- Vzťahy medzi triedami (asociácia, agregácia, kompozícia)
- Práca s kontajnermi STL, ako je std::vector
- Implementácia biznis logiky
- Použitie ukazovateľov a referencií

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

## Úlohy

{{< cards cols="2" >}}
    {{< card link="./task-2-1" title="Úloha 2.1" subtitle="Trieda knihy" icon="document" >}}
    {{< card link="./task-2-2" title="Úloha 2.2" subtitle="Práca s bankovým účtom" icon="document" >}}
    {{< card link="./task-2-3" title="Úloha 2.3" subtitle="Známkovanie študenta" icon="document" >}}
    {{< card link="./task-2-4" title="Úloha 2.4" subtitle="Objednávkový systém" icon="document" tag="Komplexné" tagType="info" >}}
    {{< card link="./task-2-5" title="Úloha 2.5" subtitle="Prihlásenie študenta na kurz" icon="document" tag="Komplexné" tagType="info" >}}
{{< /cards >}}
