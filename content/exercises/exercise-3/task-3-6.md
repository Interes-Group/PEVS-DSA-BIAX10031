---
date: '2025-03-02T14:21:28+01:00'
title: 'Úloha 3.6'
weight: 6
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte dátovú štruktúru, ktorej prvky budú vždy unikátne. Hodnoty štruktúry sú typu string. Prvky nemusia byť
usporiadané. Ak je zavolaná funkcia pre vloženie nového prvku, ktorý v štruktúre už existuje, nový prvok nie je vložený.

**Pre implementácia štruktúry použitie triedy.**

Pre štruktúru implementujte nasledovné funkcie:

- `void insert(string value)` - Vloženie nového prvku.
- `void remove(string value)` - Vymazanie prvku s danou hodnotou.
- `int size()` - Vráti aktuálnosť veľkosť štruktúry.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov zásobníka. Hodnoty prvkov zásobníku sú uvedené v
  poradí v akom sú uložené a sú oddelené čiarkou. Ak je zásobník prázdny vráti prázdny string.

Funkcie implementujte ako verejné členy triedy.

Pre demonštráciu vypracovania vytvorte ľubovolnú inštanciu štruktúry, do ktoréj postupne pridáte viacero prvkov,
zavoláte na nej funkcie `insert()` a `remove()` a `size()` v ľubovolnom poradí. Po každej úprave štruktúry vypíšte na
štandardný výstup, výstup funkcie `to_string()` pre vizuálne znázornenie funkcionality.

### Príklady volania funkcií

V nasledujúcom príklade je štruktúra uložená do premennej mnozina:

```cpp
mnozina.insert("Milan");
mnozina.insert("Jano");
mnozina.insert("Fero");
cout << mnozina.to_string() << endl; // Milan, Jano, Fero

mnozina.insert("Milan");
cout << mnozina.size() << endl; // 3
cout << mnozina.to_string() << endl; // Milan, Jano, Fero

mnozina.remove("Milan");
cout << mnozina.size() << endl; // 2
cout << mnozina.to_string() << endl; // Jano, Fero
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
