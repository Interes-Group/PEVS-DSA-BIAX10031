---
date: '2025-03-23T20:54:19+01:00'
title: 'Úloha 5.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte trieďací algoritmus **Merge sort** na STL kontajnery `list<string>`, t.j. zreťazený zoznam reťazcov.
Pre porovnanie prvkov zoznamu môžte použiť funkciu [`std::strcmp()`](https://en.cppreference.com/w/cpp/string/byte/strcmp).
Funkciu pre zotriedenie zoznamu implementujte tak aby bolo možné zadať ako parameter či má algoritmus zotriediť prvky
vzostupne (prvé sú reťazce čo lexikograficky skôr), zostupne (prvé sú reťazce, ktoré sú lexikograficky neskôr).

>[!IMPORTANT]
> Dajte si pozor na to, že STL `list` nemá prísť k prvkov cez index (t.j. nie je `list.at()` alebo `list[0]`).
> Pre prechádzanie zoznamu použite iterátor (`list.begin()` a `list.end()`). Inštancií iterátora môžte mať viacero na jeden
> zoznam.

Viac o Merge sort algoritme sa viete dozvedieť napríklad na stránkach:

- [https://en.wikipedia.org/wiki/Merge_sort](https://www.geeksforgeeks.org/merge-sort/?ref=shm)
- [https://www.programiz.com/dsa/merge-sort](https://www.geeksforgeeks.org/merge-sort/?ref=shm)
- [https://www.geeksforgeeks.org/merge-sort/?ref=shm](https://www.geeksforgeeks.org/merge-sort/?ref=shm)

### Príklady vstupov / výstupov programu

```cpp
["Milan", "Martin", "Eva"] -sort-> ["Eva","Martin","Milan"]

["Fero", "Jano", ""]       -sort-> ["", "Fero", "Jano"]
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
