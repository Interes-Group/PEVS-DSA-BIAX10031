---
date: '2025-03-02T14:21:19+01:00'
title: 'Úloha 3.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Pre implementovanie tejto úlohy môžte pokračovať s vypracovaním predchádzajúcej úlohy 3.1.

Implementujte **obojstranne zreťazený** zoznam. Vypracujte vlastnú implementáciu zreťazeného zoznamu,
ktorého prvok je hodnotu typu `string`. Prvok zoznamu obsahuje pointer na ďalší prvok zoznamu a zároveň aj prvok na
predchádzajúci prvok zoznamu. Pointer na ďalší prvok má hodnotu `NULL` ak ide o posledný prvok zoznamu. Pointer na
predchádzajúci prvok má hodnotu NULL ak ide o prvý prvok zoznamu. Zoznam implementujte bez použitia STL knižníc (
_array_, _list_, _vector_ a ďalšie).

**Pre implementácia zoznamu použitie triedy.**

Pre zoznam implementujte nasledovné funkcie:

- `string begin()` - Vráti string hodnotu prvého prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string end()` - Vráti string hodnotu posledného prvku zoznamu. Ak je zoznam prázdny vráti prázdny string.
- `string at(int index)` - Vráti string hodnotu prvku na definovanom indexe v zozname. Zoznam je indexovaný od 0. Ak
  index v zozname neexistuje vráti prázdny string.
- `void insert(int index, string value)` - Vloží novú string hodnotu na definovaný index v zozname. Ak je index väčší
  ako veľkosť zoznamu vloží nový prvok na koniec zoznamu.
- `void remove(int index)` - Vymaže prvok z definovaného indexu v zozname. Ak index v zozname neexistuje funkcia neurobí
  nič.
- `string to_string()` - Vráti string reprezentáciu všetkých prvkov zoznamu. Hodnoty prvkov zoznamu sú uvedené v poradí
  v akom sú uložené a sú oddelené čiarkou. Ak je zoznam prázdny vráti prázdny string.

**Funkcie môžte implementovať** samostatne (vtedy ešte pridajte argument pre zoznam samotný), alebo **ako členy triedy (
odporúčaný spôsob).**

Pre demonštráciu vypracovania vytvorte ľubovolný zoznam, do ktorého postupne pridáte viacero prvkov, zavoláte na ňom
funkcie `begin()` a `end()` a potom funkcie `remove()` a `insert()` v ľubovolnom poradí. Po každej úprave zoznamu
vypíšte na
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

<!--
```cpp
#include <iostream>
#include <string>

using namespace std;

// Trieda reprezentujúca uzol v obojstranne zreťazenom zozname
class Node {
public:
    string value; // Hodnota uzla
    Node* next; // Ukazovateľ na ďalší uzol
    Node* prev; // Ukazovateľ na predchádzajúci uzol

    // Konštruktor inicializuje hodnotu a nastaví ukazovatele na nullptr
    Node(string val) : value(val), next(nullptr), prev(nullptr) {}
};

// Trieda reprezentujúca obojstranne zreťazený zoznam
class DoublyLinkedList {
private:
    Node* head; // Ukazovateľ na prvý uzol zoznamu
    Node* tail; // Ukazovateľ na posledný uzol zoznamu

public:
    DoublyLinkedList() : head(nullptr), tail(nullptr) {} // Konštruktor inicializuje prázdny zoznam

    // Dekštruktor, ktorý uvoľní všetky uzly zoznamu
    ~DoublyLinkedList() {
        Node* current = head;
        while (current) {
            Node* temp = current;
            current = current->next;
            delete temp;
        }
    }

    // Funkcia vráti hodnotu prvého uzla, ak existuje, inak vráti prázdny reťazec
    string begin() {
        return head ? head->value : "";
    }

    // Funkcia vráti hodnotu posledného uzla, ak existuje, inak vráti prázdny reťazec
    string end() {
        return tail ? tail->value : "";
    }

    // Funkcia vráti hodnotu uzla na danom indexe, ak existuje, inak vráti prázdny reťazec
    string at(int index) {
        Node* current = head;
        int count = 0;
        while (current) {
            if (count == index) return current->value;
            current = current->next;
            count++;
        }
        return "";
    }

    // Vloží nový uzol na daný index. Ak index neexistuje, pridá uzol na koniec.
    void insert(int index, string value) {
        Node* newNode = new Node(value);
        if (!head) { // Ak je zoznam prázdny, nový uzol sa stáva hlavou aj chvostom
            head = tail = newNode;
            return;
        }
        
        if (index <= 0) { // Vloženie na začiatok
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
            return;
        }
        
        Node* current = head;
        int count = 0;
        while (current->next && count < index - 1) { // Posun na správnu pozíciu
            current = current->next;
            count++;
        }
        newNode->next = current->next;
        newNode->prev = current;
        if (current->next) {
            current->next->prev = newNode;
        } else {
            tail = newNode;
        }
        current->next = newNode;
    }

    // Odstráni uzol na danom indexe, ak existuje
    void remove(int index) {
        if (!head) return;
        if (index == 0) { // Odstránenie prvého uzla
            Node* temp = head;
            head = head->next;
            if (head) head->prev = nullptr;
            else tail = nullptr;
            delete temp;
            return;
        }
        
        Node* current = head;
        int count = 0;
        while (current->next && count < index - 1) { // Posun na uzol pred tým, ktorý chceme odstrániť
            current = current->next;
            count++;
        }
        if (current->next) { // Ak existuje uzol na odstránenie
            Node* temp = current->next;
            current->next = temp->next;
            if (temp->next) temp->next->prev = current;
            else tail = current;
            delete temp;
        }
    }

    // Funkcia vráti reťazcovú reprezentáciu zoznamu oddelenú čiarkami
    string to_string() {
        if (!head) return "";
        string result = "";
        Node* current = head;
        while (current) {
            result += current->value;
            if (current->next) result += ", ";
            current = current->next;
        }
        return result;
    }
};

int main() {
    DoublyLinkedList zoznam;

    // Vkladanie prvkov na koniec zoznamu
    zoznam.insert(99, "Milan");
    zoznam.insert(99, "Jano");
    zoznam.insert(99, "Fero");
    cout << zoznam.to_string() << endl; // Milan, Jano, Fero

    // Výpis prvého a posledného prvku
    cout << "First: " << zoznam.begin() << endl; // Milan
    cout << "Last: " << zoznam.end() << endl; // Fero
    cout << "At index 1: " << zoznam.at(1) << endl; // Jano

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
