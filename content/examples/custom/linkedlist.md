---
date: '2026-03-10T00:11:44+01:00'
title: 'LinkedList'
---

Tento program v jazyku **C++** implementuje **dvojito viazaný zoznam** (`doubly linked list`) pre textové hodnoty typu
`string`.

Implementácia najmä demonštruje:

- ako fungujú **triedy a objekty**,
- ako sa pracuje s **ukazovateľmi**,
- ako sa dynamicky vytvárajú prvky pomocou `new`,
- ako sa medzi sebou prepájajú uzly zoznamu cez odkazy `next` a `previous`,
- ako sa robia základné operácie nad zoznamom:
    - pridanie prvku,
    - odstránenie posledného prvku,
    - odstránenie konkrétneho prvku podľa hodnoty,
    - prechod zoznamom a výpis.

Program teda slúži ako jednoduchá ukážka dátovej štruktúry **lineárneho zoznamu**, kde každý prvok pozná svojho *
*nasledovníka aj predchodcu**.

```cpp
#include <iostream>

using namespace std;

class ListItem {
private:
    string value;
    ListItem *next = nullptr;
    ListItem *previous = nullptr;

public:
    ListItem(const string &value) {
        this->value = value;
    }

    string getValue() {
        return this->value;
    }

    ListItem *getNext() const {
        return this->next;
    }

    void setNext(ListItem *list_item) {
        this->next = list_item;
    }

    ListItem *getPrevious() const {
        return this->previous;
    }

    void setPrevious(ListItem *previous) {
        this->previous = previous;
    }
};

class List {
private:
    ListItem *head = nullptr; // NULL

public:
    List() = default;

    List(const string &value) {
        this->push(value);
    }

    void push(const string &value) {
        if (this->head == nullptr) {
            this->head = new ListItem(value);
            return;
        }
        ListItem *tail = this->find_tail();
        ListItem *newItem = new ListItem(value);
        tail->setNext(newItem);
        newItem->setPrevious(tail);
    }

    void pop() {
        if (this->head == nullptr) return;
        ListItem *tail = this->find_tail();
        ListItem *preTail = tail->getPrevious();
        if (preTail != nullptr) {
            preTail->setNext(nullptr);
        }
        if (this->head == tail) {
            this->head = nullptr;
        }
        delete tail;
    }

    void pop(const string &value) {
        if (this->head == nullptr) return;
        ListItem *current = this->head;
        ListItem *previous = current->getPrevious();
        while (current != nullptr && current->getValue() != value) {
            previous = current;
            current = current->getNext();
        }
        if (current == nullptr) return;
        if (previous != nullptr) {
            previous->setNext(current->getNext());
            if (current->getNext() != nullptr) {
                current->getNext()->setPrevious(previous);
            }
        }
        if (current == this->head) {
            this->head = this->head->getNext();
        }
        delete current;
    }

    ListItem *find_tail() const {
        ListItem *current = this->head;
        if (current == nullptr) {
            return nullptr;
        }
        while (current->getNext() != nullptr) {
            current = current->getNext();
        }
        return current;
    }

    string to_string() const {
        string output;
        ListItem *current = this->head;
        while (current != nullptr) {
            output += current->getValue() + ", ";
            current = current->getNext();
        }
        return output;
    }
};

int main() {
    List *list = new List("1");
    cout << "List: " << list->to_string() << endl;
    list->push("2");
    list->push("3");
    list->pop("2");

    cout << "List: " << list->to_string() << endl;

    return 0;
}
```

# Vysvetlenie

## 1. Trieda `ListItem`

```c++
class ListItem {
private:
    string value;
    ListItem *next = nullptr;
    ListItem *previous = nullptr;
```

Táto trieda predstavuje **jeden prvok zoznamu**.

Každý prvok obsahuje:

- `value` – uloženú hodnotu,
- `next` – ukazovateľ na ďalší prvok,
- `previous` – ukazovateľ na predchádzajúci prvok.

Keďže ide o **dvojito viazaný zoznam**, každý uzol vie ísť:

- dopredu cez `next`,
- dozadu cez `previous`.

### Prečo `nullptr`?

Na začiatku prvok nemusí mať suseda, preto sú ukazovatele nastavené na `nullptr`, čo znamená „na nič neukazuje“.

---

### 2. Konštruktor triedy `ListItem`

```c++
public:
    ListItem(const string &value) {
        this->value = value;
    }
```

Konštruktor sa zavolá pri vytváraní nového prvku.

#### `const string &value`

Hodnota sa odovzdáva:

- ako `const` – nebude sa meniť,
- ako referencia `&` – zbytočne sa nekopíruje.

#### `this->value = value;`

Kľúčové slovo `this` odkazuje na aktuálny objekt.  
Teda hovoríme: „ulož prijatú hodnotu do členskej premennej objektu“.

---

### 3. Getter a setter metódy v `ListItem`

#### Získanie hodnoty

```c++
string getValue() {
    return this->value;
}
```

Vráti text uložený v danom prvku.

---

#### Získanie nasledujúceho prvku

```c++
ListItem *getNext() const {
    return this->next;
}
```

Vráti ukazovateľ na ďalší prvok v zozname.

---

#### Nastavenie nasledujúceho prvku

```c++
void setNext(ListItem *list_item) {
    this->next = list_item;
}
```

Nastaví, na ktorý prvok má ukazovať `next`.

---

#### Získanie predchádzajúceho prvku

```c++
ListItem *getPrevious() const {
    return this->previous;
}
```

Vráti ukazovateľ na predchádzajúci prvok.

---

#### Nastavenie predchádzajúceho prvku

```c++
void setPrevious(ListItem *previous) {
    this->previous = previous;
}
```

Nastaví, na ktorý prvok má ukazovať `previous`.

---

## 4. Trieda `List`

```c++
class List {
private:
    ListItem *head = nullptr; // NULL
```

Táto trieda predstavuje **celý zoznam**.

Obsahuje len jednu hlavnú členskú premennú:

- `head` – ukazovateľ na **prvý prvok zoznamu**.

Ak je `head == nullptr`, zoznam je prázdny.

---

## 5. Konštruktory triedy `List`

### Predvolený konštruktor

```c++
public:
    List() = default;
```

Tento konštruktor vytvorí prázdny zoznam.

---

### Konštruktor s počiatočnou hodnotou

```c++
List(const string &value) {
    this->push(value);
}
```

Tento konštruktor vytvorí zoznam a hneď doň vloží prvý prvok.

To znamená, že:

```c++
List("1");
```

vytvorí zoznam, ktorý už obsahuje jeden prvok s hodnotou `"1"`.

---

## 6. Metóda `push` – pridanie prvku na koniec

```c++
void push(const string &value) {
    if (this->head == nullptr) {
        this->head = new ListItem(value);
        return;
    }
    ListItem *tail = this->find_tail();
    ListItem *newItem = new ListItem(value);
    tail->setNext(newItem);
    newItem->setPrevious(tail);
}
```

Táto metóda pridáva nový prvok **na koniec zoznamu**.

### Ako funguje?

#### Prípad 1: zoznam je prázdny

```c++
if (this->head == nullptr) {
    this->head = new ListItem(value);
    return;
}
```

Ak neexistuje prvý prvok, nový prvok sa stane `head`.

---

#### Prípad 2: zoznam už obsahuje prvky

```c++
ListItem *tail = this->find_tail();
ListItem *newItem = new ListItem(value);
tail->setNext(newItem);
newItem->setPrevious(tail);
```

Postup:

1. nájde sa posledný prvok (`tail`),
2. vytvorí sa nový prvok,
3. starý posledný prvok nastaví svoj `next` na nový prvok,
4. nový prvok nastaví svoj `previous` na starý posledný prvok.

Takto sa zachová správne prepojenie oboma smermi.

---

## 7. Metóda `pop()` – odstránenie posledného prvku

```c++
void pop() {
    if (this->head == nullptr) return;
    ListItem *tail = this->find_tail();
    ListItem *preTail = tail->getPrevious();
    if (preTail != nullptr) {
        preTail->setNext(nullptr);
    }
    if (this->head == tail) {
        this->head = nullptr;
    }
    delete tail;
}
```

Táto verzia `pop()` odstráni **posledný prvok zoznamu**.

### Postup:

#### 1. Ak je zoznam prázdny, nič sa nestane

```c++
if (this->head == nullptr) return;
```

---

#### 2. Nájde sa posledný prvok

```c++
ListItem *tail = this->find_tail();
```

---

#### 3. Zistí sa prvok pred ním

```c++
ListItem *preTail = tail->getPrevious();
```

---

#### 4. Ak predposledný prvok existuje, jeho `next` sa nastaví na `nullptr`

```c++
if (preTail != nullptr) {
    preTail->setNext(nullptr);
}
```

Tým sa posledný prvok odpojí zo zoznamu.

---

#### 5. Ak bol v zozname iba jeden prvok

```c++
if (this->head == tail) {
    this->head = nullptr;
}
```

Po odstránení už zoznam nebude mať žiadny prvok.

---

#### 6. Uvoľnenie pamäte

```c++
delete tail;
```

Keďže prvok bol vytvorený cez `new`, musí sa zmazať cez `delete`.

---

## 8. Metóda `pop(const string &value)` – odstránenie prvku podľa hodnoty

```c++
void pop(const string &value) {
    if (this->head == nullptr) return;
    ListItem *current = this->head;
    ListItem *previous = current->getPrevious();
    while (current != nullptr && current->getValue() != value) {
        previous = current;
        current = current->getNext();
    }
    if (current == nullptr) return;
    if (previous != nullptr) {
        previous->setNext(current->getNext());
        if (current->getNext() != nullptr) {
            current->getNext()->setPrevious(previous);
        }
    }
    if (current == this->head) {
        this->head = this->head->getNext();
    }
    delete current;
}
```

Táto metóda odstráni **prvý nájdený prvok**, ktorého hodnota sa rovná zadanému textu.

---

### Krok po kroku

#### 1. Kontrola prázdneho zoznamu

```c++
if (this->head == nullptr) return;
```

Ak zoznam neobsahuje nič, nie je čo mazať.

---

#### 2. Nastavenie pomocných ukazovateľov

```c++
ListItem *current = this->head;
ListItem *previous = current->getPrevious();
```

- `current` – aktuálne prezeraný prvok,
- `previous` – prvok pred ním.

Na začiatku je `current` nastavený na prvý prvok a jeho `previous` bude väčšinou `nullptr`.

---

#### 3. Hľadanie prvku s požadovanou hodnotou

```c++
while (current != nullptr && current->getValue() != value) {
    previous = current;
    current = current->getNext();
}
```

Cyklus ide po zozname dovtedy, kým:

- nenarazí na koniec,
- alebo nenájde zhodnú hodnotu.

---

#### 4. Ak sa prvok nenašiel

```c++
if (current == nullptr) return;
```

Nič sa neodstraňuje.

---

#### 5. Ak prvok nie je prvý, prepojí sa okolie

```c++
if (previous != nullptr) {
    previous->setNext(current->getNext());
    if (current->getNext() != nullptr) {
        current->getNext()->setPrevious(previous);
    }
}
```

Toto je jadro mazania:

- predchádzajúci prvok začne ukazovať na nasledujúci,
- nasledujúci prvok začne ukazovať späť na predchádzajúci.

Tak sa odstránený prvok „vystrihne“ zo zoznamu.

---

#### 6. Ak sa maže prvý prvok

```c++
if (current == this->head) {
    this->head = this->head->getNext();
}
```

Ak bol mazaný prvok `head`, treba posunúť začiatok zoznamu na ďalší prvok.

> Tu je malá dôležitá poznámka: po presunutí `head` by bolo dobré ešte nastaviť nový `head->previous = nullptr`, ak nový
`head` existuje.  
> Inak môže ostať spätne prepojený na už zmazaný prvok.  
> Program v tomto jednoduchom príklade môže fungovať, ale z pohľadu korektnosti je to slabšie miesto.

---

#### 7. Uvoľnenie pamäte

```c++
delete current;
```

Odstránený prvok sa vymaže z pamäte.

---

## 9. Metóda `find_tail()` – nájdenie posledného prvku

```c++
ListItem *find_tail() const {
    ListItem *current = this->head;
    if (current == nullptr) {
        return nullptr;
    }
    while (current->getNext() != nullptr) {
        current = current->getNext();
    }
    return current;
}
```

Táto metóda nájde **posledný prvok zoznamu**.

### Ako funguje?

1. začne od `head`,
2. postupuje cez `next`,
3. skončí pri prvku, ktorého `next == nullptr`.

Takýto prvok je koniec zoznamu, teda `tail`.

Ak je zoznam prázdny, vráti `nullptr`.

---

## 10. Metóda `to_string()` – prevedenie zoznamu na text

```c++
string to_string() const {
    string output;
    ListItem *current = this->head;
    while (current != nullptr) {
        output += current->getValue() + ", ";
        current = current->getNext();
    }
    return output;
}
```

Táto metóda prejde celý zoznam a spojí hodnoty do jedného reťazca.

### Príklad:

Ak zoznam obsahuje hodnoty:

- `"1"`
- `"2"`
- `"3"`

výsledok bude približne:

```c++
"1, 2, 3, "
```

### Poznámka

Na konci ostane navyše `", "`.  
Nie je to chyba, ale výstup nie je úplne estetický. Pre študijný príklad je to v poriadku.

---

## 11. Funkcia `main()` – ukážka použitia programu

```c++
int main() {
    List *list = new List("1");
    cout << "List: " << list->to_string() << endl;
    list->push("2");
    list->push("3");
    list->pop("2");

    cout << "List: " << list->to_string() << endl;

    return 0;
}
```

Toto je vstupný bod programu.

---

### Krok 1: vytvorenie zoznamu

```c++
List *list = new List("1");
```

Vytvorí sa nový zoznam s jedným prvkom `"1"`.

---

### Krok 2: prvý výpis

```c++
cout << "List: " << list->to_string() << endl;
```

Na obrazovku sa vypíše aktuálny obsah zoznamu.

Výstup bude približne:

```c++
List: 1,
```

presnejšie s medzerou a čiarkou podľa implementácie.

---

### Krok 3: pridanie ďalších prvkov

```c++
list->push("2");
list->push("3");
```

Zoznam sa rozšíri na:

```c++
1 -> 2 -> 3
```

a zároveň spätné väzby budú:

```c++
3 <- 2 <- 1
```

---

### Krok 4: odstránenie hodnoty `"2"`

```c++
list->pop("2");
```

Zo zoznamu sa odstráni prvok s hodnotou `"2"`.

Po tejto operácii bude zoznam obsahovať:

```c++
1 -> 3
```

---

### Krok 5: druhý výpis

```c++
cout << "List: " << list->to_string() << endl;
```

Výstup bude približne:

```c++
List: 1, 3,
```

---

## 12. Zhrnutie

Program demonštruje implementáciu **dvojito viazaného zoznamu** v C++.

Vie:

- vytvoriť zoznam,
- pridať prvok na koniec,
- odstrániť posledný prvok,
- odstrániť prvok podľa hodnoty,
- vypísať obsah zoznamu.

Na študijné účely je užitočný najmä preto, že ukazuje prácu s:

- ukazovateľmi,
- dynamickou pamäťou,
- prepojením objektov,
- návrhom tried.

Ak chceš, môžem ti hneď potom napísať aj:

1. **kratšiu verziu vhodnú do dokumentácie alebo odovzdania**, alebo
2. **riadok po riadku komentovaný kód v slovenčine**.