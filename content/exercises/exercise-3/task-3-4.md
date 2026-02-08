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

<!--

```cpp
#include <iostream>
#include <string>

using namespace std;

// Trieda reprezentujúca uzol v zásobníku
class Node {
public:
    string value; // Hodnota uzla
    Node* next; // Ukazovateľ na ďalší uzol (nižšie v zásobníku)

    // Konštruktor inicializuje hodnotu a nastaví ukazovateľ na nullptr
    Node(string val) : value(val), next(nullptr) {}
};

// Trieda reprezentujúca zásobník (LIFO)
class Stack {
private:
    Node* topNode; // Ukazovateľ na vrchný uzol zásobníka
    int count; // Počet prvkov v zásobníku

public:
    Stack() : topNode(nullptr), count(0) {} // Konštruktor inicializuje prázdny zásobník

    // Dekštruktor, ktorý uvoľní všetky uzly zásobníka
    ~Stack() {
        while (topNode) {
            Node* temp = topNode;
            topNode = topNode->next;
            delete temp;
        }
    }

    // Funkcia vloží prvok na vrch zásobníka
    void push(string value) {
        Node* newNode = new Node(value);
        newNode->next = topNode;
        topNode = newNode;
        count++;
    }

    // Funkcia odstráni a vráti vrchný prvok zásobníka
    string pop() {
        if (!topNode) return "";
        Node* temp = topNode;
        string value = temp->value;
        topNode = topNode->next;
        delete temp;
        count--;
        return value;
    }

    // Funkcia vráti hodnotu vrchného prvku zásobníka bez jeho odstránenia
    string top() {
        return topNode ? topNode->value : "";
    }

    // Funkcia vráti počet prvkov v zásobníku
    int size() {
        return count;
    }

    // Funkcia vráti reťazcovú reprezentáciu zásobníka (hodnoty oddelené čiarkou)
    string to_string() {
        if (!topNode) return "";
        string result = "";
        Node* current = topNode;
        while (current) {
            result += current->value;
            if (current->next) result += ", ";
            current = current->next;
        }
        return result;
    }
};

int main() {
    Stack zasobnik;

    // Vkladanie prvkov do zásobníka
    zasobnik.push("Milan");
    zasobnik.push("Jano");
    zasobnik.push("Fero");
    cout << zasobnik.to_string() << endl; // Fero, Jano, Milan

    // Výpis vrchného prvku a veľkosti zásobníka
    cout << "Top: " << zasobnik.top() << endl; // Fero
    cout << "Size: " << zasobnik.size() << endl; // 3

    // Odstránenie prvku zo zásobníka
    cout << "Pop: " << zasobnik.pop() << endl; // Fero
    cout << zasobnik.to_string() << endl; // Jano, Milan

    // Ďalšie odstránenie prvku
    cout << "Pop: " << zasobnik.pop() << endl; // Jano
    cout << "Top: " << zasobnik.top() << endl; // Milan
    cout << zasobnik.to_string() << endl; // Milan

    return 0;
}
```

-->
{{< /details >}}
