---
date: '2025-03-02T14:21:15+01:00'
title: 'Úloha 3.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte **jednosmerne/dopredne zreťazený** zoznam. Vypracujte vlastnú implementáciu zreťazeného zoznamu,
ktorého prvok je hodnotu typu `string`. Prvok zoznamu obsahuje pointer na ďalší prvok zoznamu. Pointer na ďalší prvok má
hodnotu `NULL` ak ide o posledný prvok zoznamu. Zoznam implementujte bez použitia STL knižníc (_array_, _list_, _vector_ a
ďalšie).

**Pre implementácia zoznamu použitie triedy.**

Pre zoznam implementujte nasledovné funkcie:

- `string begin()` - Vráti string hodnotu prvého prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string end()` - Vráti string hodnotu posledného prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string at(int index)` - Vráti string hodnotu prvku na definovanom indexe v zozname. Zoznam je indexovaný od 0. Ak index
  v zozname neexistuje vráti prázdny string.
- `void insert(int index, string value)` - Vloží novú string hodnotu na definovaný index v zozname. Ak je index väčší ako
  veľkosť zoznamu vloží nový prvok na koniec zoznamu.
- `void remove(int index)` - Vymaže prvok z definovaného indexu v zozname. Ak index v zozname neexistuje funkcia neurobí
  nič.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov zoznamu. Hodnoty prvkov zoznamu sú uvedené v poradí v
  akom sú uložené a sú oddelené čiarkou. Ak je zoznam prázdny vráti prázdny string.

**Funkcie môžte implementovať** samostatne (vtedy ešte pridajte argument pre zoznam samotný), alebo **ako členy triedy (
odporúčaný spôsob).**

Pre demonštráciu vypracovania vytvorte ľubovolný zoznam, do ktorého postupne pridáte viacero prvkov, zavoláte na ňom
funkcie `begin()` a `end()` a potom funkcie `remove()` a `insert()` v ľubovolnom poradí. Po každej úprave zoznamu vypíšte na
štandardný výstup, výstup funkcie `to_string()` pre vizuálne znázornenie funkcionality.

### Príklady volania funkcií

V nasledujúcom príklade je zoznam uložený do premennej zoznam:

```cpp
zoznam.insert(99,"Milan");
zoznam.insert(99,"Jano");
zoznam.insert(99,"Fero");
cout << zoznam.to_string() << endl; // Milan, Jano, Fero

cout << zoznam.begin() << endl; // Milan
cout << zoznam.end() << endl; // Fero
cout << zoznam.at(1) << endl; // Jano

zoznam.remove(1);
cout << zoznam.to_string() << endl; // Milan, Fero
zoznam.insert(1,"Mária");
cout << zoznam.to_string() << endl; // Milan, Mária, Fero
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
