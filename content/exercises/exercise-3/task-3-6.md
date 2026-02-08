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

<!--

```cpp
#include <iostream>
#include <string>

using namespace std;

// Trieda reprezentujúca uzol v množine unikátnych prvkov
class Node {
public:
    string value; // Hodnota uzla
    Node* next; // Ukazovateľ na ďalší uzol v množine

    // Konštruktor inicializuje hodnotu a nastaví ukazovateľ na nullptr
    Node(string val) : value(val), next(nullptr) {}
};

// Trieda reprezentujúca množinu unikátnych prvkov
class UniqueSet {
private:
    Node* headNode; // Ukazovateľ na prvý uzol v množine
    int count; // Počet prvkov v množine

public:
    UniqueSet() : headNode(nullptr), count(0) {} // Konštruktor inicializuje prázdnu množinu

    // Dekštruktor, ktorý uvoľní všetky uzly množiny
    ~UniqueSet() {
        while (headNode) {
            Node* temp = headNode;
            headNode = headNode->next;
            delete temp;
        }
    }

    // Funkcia vloží nový unikátny prvok do množiny
    void insert(string value) {
        if (exists(value)) return; // Zabezpečenie unikátnosti
        Node* newNode = new Node(value);
        newNode->next = headNode;
        headNode = newNode;
        count++;
    }

    // Funkcia odstráni prvok s danou hodnotou
    void remove(string value) {
        if (!headNode) return;
        
        if (headNode->value == value) {
            Node* temp = headNode;
            headNode = headNode->next;
            delete temp;
            count--;
            return;
        }
        
        Node* current = headNode;
        while (current->next && current->next->value != value) {
            current = current->next;
        }
        
        if (current->next) {
            Node* temp = current->next;
            current->next = current->next->next;
            delete temp;
            count--;
        }
    }

    // Funkcia vráti počet prvkov v množine
    int size() {
        return count;
    }

    // Funkcia vráti reťazcovú reprezentáciu množiny (hodnoty oddelené čiarkou)
    string to_string() {
        if (!headNode) return "";
        string result = "";
        Node* current = headNode;
        while (current) {
            result += current->value;
            if (current->next) result += ", ";
            current = current->next;
        }
        return result;
    }

private:
    // Pomocná funkcia na kontrolu existencie prvku v množine
    bool exists(string value) {
        Node* current = headNode;
        while (current) {
            if (current->value == value) return true;
            current = current->next;
        }
        return false;
    }
};

int main() {
    UniqueSet mnozina;

    // Vkladanie prvkov do množiny
    mnozina.insert("Milan");
    mnozina.insert("Jano");
    mnozina.insert("Fero");
    cout << mnozina.to_string() << endl; // Milan, Jano, Fero

    // Pokus o vkladanie duplicitného prvku
    mnozina.insert("Milan");
    cout << "Size: " << mnozina.size() << endl; // 3
    cout << mnozina.to_string() << endl; // Milan, Jano, Fero

    // Odstránenie prvku z množiny
    mnozina.remove("Milan");
    cout << "Size: " << mnozina.size() << endl; // 2
    cout << mnozina.to_string() << endl; // Jano, Fero

    return 0;
}
```

-->
{{< /details >}}
