---
date: '2025-03-02T14:21:23+01:00'
title: 'Úloha 3.4'
weight: 4
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte dátovú štruktúru podľa princípu **LIFO** (Last in First out - posledný vložený je prvý vybraný), t.j. **zásobník**
alebo stack. Prvky štruktúry uchovávajú hodnotu typu `string`. Zásobník implementujte bez použitia STL knižníc (_stack_, _queue_ a ďalšie).

**Pre implementácia zásobníka použitie triedy.**

Pre zásobník implementujte nasledovné funkcie:

- `void push(string value)` - Vloží prvok do zásobníka.
- `string pop()` - Vyberie prvok zo zásobníka. Hodnota prvku je vrátená funkciou.
- `string top()` - Vrátiť hodnotu vrchného prvku zásobníka. Toto je prvok, ktorý bude vybratý pri najbližšom volaní funkcie `pop()`.
- `int size()` - Vráti aktuálnu veľkosť zásobníka.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov zásobníka. Hodnoty prvkov zásobníku sú uvedené v poradí
    v akom sú uložené a sú oddelené čiarkou. Ak je zásobník prázdny vráti prázdny string.

Funkcie implementujte ako verejné členy triedy.

Pre demonštráciu vypracovania vytvorte ľubovolný zásobník, do ktorého postupne pridáte viacero prvkov, zavoláte na ňom
funkcie `push()` a `pop()` a potom funkcie `size()` a `top()` v ľubovolnom poradí. Po každej úprave zásobníka
vypíšte na štandardný výstup, výstup funkcie `to_string()` pre vizuálne znázornenie funkcionality.

### Príklady volania funkcií

V nasledujúcom príklade je zásobník uložený do premennej zasobnik:

```cpp
zasobnik.push("Milan");
zasobnik.push("Jano");
zasobnik.push("Fero");
cout << zasobnik.to_string() << endl; // Fero, Jano, Milan

cout << zasobnik.top() << endl; // Fero
cout << zasobnik.size() << endl; // 3

cout << zasobnik.pop() << endl; // Fero
cout << zasobnik.to_string() << endl; // Jano, Milan

cout << zasobnik.pop() << endl; // Jano
cout << zasobnik.top() << endl; // Milan
cout << zasobnik.to_string() << endl; // Milan
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
