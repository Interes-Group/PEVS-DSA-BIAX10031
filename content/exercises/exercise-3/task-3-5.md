---
date: '2025-03-02T14:21:25+01:00'
title: 'Úloha 3.5'
weight: 5
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť. // fronta

Implementujte dátovú štruktúru podľa princípu **FIFO** (First in First out - prvý vložený je prvý vybraný), t.j. **fronta** 
alebo queue. Prvky štruktúry uchovávajú hodnotu typu `string`. Frontu implementujte bez použitia STL knižníc (_stack_,
_queue_ a ďalšie).

**Pre implementácia fronty použitie triedy.**

Pre frontu implementujte nasledovné funkcie:

- `void push(string value)` - Vloží prvok do fronty.
- `string pop()` - Vyberie prvok z fronty. Hodnota prvku je vrátená funkciou.
- `string head()` - Vráti hodnotu vrchného prvku fronty. Toto je prvok, ktorý bude vybratý pri najbližšom volaní funkcie
  pop().
- `string tail()` - Vráti hodnotu posledného prvku fronty. Toto je prvok, ktorý bol posledný vložený do fronty.
- `int size()` - Vráti aktuálnu veľkosť fronty.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov fronty. Hodnoty prvkov fronty sú uvedené v poradí
  v akom sú uložené a sú oddelené čiarkou. Ak je fronta prázdna vráti prázdny string.

Funkcie implementujte ako verejné členy triedy.

Pre demonštráciu vypracovania vytvorte ľubovolný frontu, do ktoréj postupne pridáte viacero prvkov, zavoláte na nej
funkcie `push()` a `pop()` a potom funkcie `size()`, `head()` a `tail()` v ľubovolnom poradí. Po každej úprave fronty
vypíšte na štandardný výstup, výstup funkcie `to_string()` pre vizuálne znázornenie funkcionality.

### Príklady volania funkcií

V nasledujúcom príklade je fronta uložený do premennej fronta:

```cpp
fronta.push("Milan");
fronta.push("Jano");
fronta.push("Fero");
cout << fronta.to_string() << endl; // Milan, Jano, Fero

cout << fronta.head() << endl; // Milan
cout << fronta.size() << endl; // 3

cout << fronta.pop() << endl; // Milan
cout << fronta.to_string() << endl; // Jano, Fero
cout << fronta.tail() << endl; // Fero

cout << fronta.pop() << endl; // Jano
cout << fronta.top() << endl; // Fero
cout << fronta.to_string() << endl; // Fero
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <string>

using namespace std;

// Trieda reprezentujúca uzol vo fronte
class Node {
public:
    string value; // Hodnota uzla
    Node* next; // Ukazovateľ na ďalší uzol vo fronte

    // Konštruktor inicializuje hodnotu a nastaví ukazovateľ na nullptr
    Node(string val) : value(val), next(nullptr) {}
};

// Trieda reprezentujúca frontu (FIFO)
class Queue {
private:
    Node* headNode; // Ukazovateľ na prvý uzol vo fronte
    Node* tailNode; // Ukazovateľ na posledný uzol vo fronte
    int count; // Počet prvkov vo fronte

public:
    Queue() : headNode(nullptr), tailNode(nullptr), count(0) {} // Konštruktor inicializuje prázdnu frontu

    // Dekštruktor, ktorý uvoľní všetky uzly fronty
    ~Queue() {
        while (headNode) {
            Node* temp = headNode;
            headNode = headNode->next;
            delete temp;
        }
    }

    // Funkcia vloží prvok na koniec fronty
    void push(string value) {
        Node* newNode = new Node(value);
        if (!tailNode) {
            headNode = tailNode = newNode;
        } else {
            tailNode->next = newNode;
            tailNode = newNode;
        }
        count++;
    }

    // Funkcia odstráni a vráti prvý prvok fronty
    string pop() {
        if (!headNode) return "";
        Node* temp = headNode;
        string value = temp->value;
        headNode = headNode->next;
        if (!headNode) tailNode = nullptr;
        delete temp;
        count--;
        return value;
    }

    // Funkcia vráti hodnotu prvého prvku fronty bez jeho odstránenia
    string head() {
        return headNode ? headNode->value : "";
    }

    // Funkcia vráti hodnotu posledného prvku fronty
    string tail() {
        return tailNode ? tailNode->value : "";
    }

    // Funkcia vráti počet prvkov vo fronte
    int size() {
        return count;
    }

    // Funkcia vráti reťazcovú reprezentáciu fronty (hodnoty oddelené čiarkou)
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
};

int main() {
    Queue fronta;

    // Vkladanie prvkov do fronty
    fronta.push("Milan");
    fronta.push("Jano");
    fronta.push("Fero");
    cout << fronta.to_string() << endl; // Milan, Jano, Fero

    // Výpis prvého prvku a veľkosti fronty
    cout << "Head: " << fronta.head() << endl; // Milan
    cout << "Size: " << fronta.size() << endl; // 3

    // Odstránenie prvku z fronty
    cout << "Pop: " << fronta.pop() << endl; // Milan
    cout << fronta.to_string() << endl; // Jano, Fero
    cout << "Tail: " << fronta.tail() << endl; // Fero

    // Ďalšie odstránenie prvku
    cout << "Pop: " << fronta.pop() << endl; // Jano
    cout << "Head: " << fronta.head() << endl; // Fero
    cout << fronta.to_string() << endl; // Fero

    return 0;
}
```

{{< /details >}}
