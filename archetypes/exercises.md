---
date: '{{ .Date }}'
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
type: 'exercise'
---

// TODO popis

{{< pdf "./pevs-dsa-XXX.pdf" >}}

### Náplň

- // TODO čo sú tu za cvičenia

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
    {{< card link="./task-X-X" title="Úloha X.X" subtitle="" icon="document" >}}
    {{< card link="./task-X-Y" title="Úloha X.Y" subtitle="" icon="document" tag="Komplexné" tagType="info" >}}
{{< /cards >}}
