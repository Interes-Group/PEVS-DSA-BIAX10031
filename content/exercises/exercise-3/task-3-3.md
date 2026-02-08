---
date: '2025-03-02T14:21:21+01:00'
title: 'Úloha 3.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Pre implementovanie tejto úlohy môžte pokračovať s vypracovaním predchádzajúcej úlohy 3.2.

Implementujte **cyklický obojstranne zreťazený** zoznam. Vypracujte vlastnú implementáciu zreťazeného zoznamu,
ktorého prvok je hodnotu typu `string`. Prvok zoznamu obsahuje pointer na ďalší prvok zoznamu a zároveň aj prvok na
predchádzajúci prvok zoznamu. Posledný prvok zoznamu má v pointeri na ďalší prvok prvý prvok zoznamu. Prvý prvok zoznamu
má v pointeri na predchádzajúci prvok posledný prvok zoznamu. Takouto implementáciou by mal byť docielený cyklus v
zozname. Zoznam implementujte bez použitia STL knižníc (_array_, _list_, _vector_ a ďalšie).

**Pre implementácia zoznamu použitie triedy.**

Pre zoznam implementujte nasledovné funkcie:

- `string begin()` - Vráti string hodnotu prvého prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string end()` - Vráti string hodnotu posledného prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string at(int index)` - Vráti string hodnotu prvku na definovanom indexe v zozname. Zoznam je indexovaný od 0. Ak
  index v zozname neexistuje vráti prázdny string.
- `string next(int index)` - Vráti nasledujúci prvok od prvku s uvedeným indexom. Ak index neexistuje vráti prázdny
  string.
- `void insert(int index, string value)` - Vloží novú string hodnotu na definovaný index v zozname. Ak je index väčší
  ako veľkosť zoznamu vloží nový prvok na koniec zoznamu.
- `void remove(int index)` - Vymaže prvok z definovaného indexu v zozname. Ak index v zozname neexistuje funkcia neurobí
  nič.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov zoznamu. Hodnoty prvkov zoznamu sú uvedené v poradí
  v akom sú uložené a sú oddelené čiarkou. Ak je zoznam prázdny vráti prázdny string. **Pozor pri prechádzaní zoznamu na
  cyklus.**

Funkcie implementujte ako verejné členy triedy.

Pre demonštráciu vypracovania vytvorte ľubovolný zoznam, do ktorého postupne pridáte viacero prvkov, zavoláte na ňom
funkcie `begin()` a `end()` a potom funkcie `remove()` a `insert()` v ľubovolnom poradí. Po každej úprave zoznamu
vypíšte na štandardný výstup, výstup funkcie `to_string()` pre vizuálne znázornenie funkcionality.

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

cout << zoznam.next(2) << endl; // Milan

zoznam.remove(1);
cout << zoznam.to_string() << endl; // Milan, Fero
zoznam.insert(1,"Mária");
cout << zoznam.to_string() << endl; // Milan, Mária, Fero
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

// Trieda reprezentujúca uzol v cyklickom obojstranne zreťazenom zozname
class Node {
public:
    string value; // Hodnota uzla
    Node* next; // Ukazovateľ na ďalší uzol
    Node* prev; // Ukazovateľ na predchádzajúci uzol

    // Konštruktor inicializuje hodnotu a nastaví ukazovatele na nullptr
    Node(string val) : value(val), next(nullptr), prev(nullptr) {}
};

// Trieda reprezentujúca cyklický obojstranne zreťazený zoznam
class CircularDoublyLinkedList {
private:
    Node* head; // Ukazovateľ na prvý uzol zoznamu

public:
    CircularDoublyLinkedList() : head(nullptr) {} // Konštruktor inicializuje prázdny zoznam

    // Dekštruktor, ktorý uvoľní všetky uzly zoznamu
    ~CircularDoublyLinkedList() {
        if (!head) return;
        Node* current = head;
        do {
            Node* temp = current;
            current = current->next;
            delete temp;
        } while (current != head);
    }

    // Funkcia vráti hodnotu prvého uzla, ak existuje, inak vráti prázdny reťazec
    string begin() {
        return head ? head->value : "";
    }

    // Funkcia vráti hodnotu posledného uzla, ak existuje, inak vráti prázdny reťazec
    string end() {
        return head ? head->prev->value : "";
    }

    // Funkcia vráti hodnotu uzla na danom indexe, ak existuje, inak vráti prázdny reťazec
    string at(int index) {
        if (!head) return "";
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
            if (current == head) return ""; // Ak sa vrátime na začiatok, index je mimo rozsahu
        }
        return current->value;
    }

    // Funkcia vráti hodnotu nasledujúceho prvku od zadaného indexu
    string next(int index) {
        if (!head) return "";
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
            if (current == head) return "";
        }
        return current->next->value;
    }

    // Vloží nový uzol na daný index. Ak index neexistuje, pridá uzol na koniec.
    void insert(int index, string value) {
        Node* newNode = new Node(value);
        if (!head) { // Ak je zoznam prázdny, nový uzol sa stáva hlavou a cyklizuje sa
            head = newNode;
            head->next = head;
            head->prev = head;
            return;
        }
        
        Node* current = head;
        for (int i = 0; i < index && current->next != head; i++) {
            current = current->next;
        }
        newNode->next = current->next;
        newNode->prev = current;
        current->next->prev = newNode;
        current->next = newNode;
    }

    // Odstráni uzol na danom indexe, ak existuje
    void remove(int index) {
        if (!head) return;
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
            if (current == head) return; // Ak sa vrátime na začiatok, index je mimo rozsahu
        }
        if (current == head && current->next == head) {
            delete head;
            head = nullptr;
            return;
        }
        current->prev->next = current->next;
        current->next->prev = current->prev;
        if (current == head) head = current->next;
        delete current;
    }

    // Funkcia vráti reťazcovú reprezentáciu zoznamu oddelenú čiarkami
    string to_string() {
        if (!head) return "";
        string result = "";
        Node* current = head;
        do {
            result += current->value;
            if (current->next != head) result += ", ";
            current = current->next;
        } while (current != head);
        return result;
    }
};

int main() {
    CircularDoublyLinkedList zoznam;

    // Vkladanie prvkov na koniec zoznamu
    zoznam.insert(99, "Milan");
    zoznam.insert(99, "Jano");
    zoznam.insert(99, "Fero");
    cout << zoznam.to_string() << endl; // Milan, Jano, Fero

    // Výpis prvého a posledného prvku
    cout << "First: " << zoznam.begin() << endl; // Milan
    cout << "Last: " << zoznam.end() << endl; // Fero
    cout << "At index 1: " << zoznam.at(1) << endl; // Jano
    
    // Výpis nasledujúceho prvku od indexu 2
    cout << "Next after index 2: " << zoznam.next(2) << endl; // Milan

    // Odstránenie prvku na indexe 1
    zoznam.remove(1);
    cout << zoznam.to_string() << endl; // Milan, Fero
    
    // Vloženie nového prvku na index 1
    zoznam.insert(1, "Mária");
    cout << zoznam.to_string() << endl; // Milan, Mária, Fero

    return 0;
}
```

-->
{{< /details >}}
