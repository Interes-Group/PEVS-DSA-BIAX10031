---
date: '2025-03-21T23:40:31+01:00'
title: 'Queue'
---

Príklad implementácie jednoduchej fronty reťazcov (`string`). Úložisko prvkov fronty je implementovaná pomocou
kontajnera `vector`.

```cpp
#include <iostream>
#include <vector>

class Queue {
private:
    std::vector<std::string> values;

public:
    Queue() = default;

    ~Queue() = default;

    void push(const std::string &value) {
        values.push_back(value);
    }

    std::string pop() {
        std::string front = values.front();
        values.erase(values.begin());
        return front;
    }

    size_t size() const {
        return values.size();
    }

    std::string front() {
        return values.front();
    }

    std::string back() {
        return values.back();
    }

    std::string to_string() {
        std::string result;
        for (const std::string &value: values) {
            result += value + " ";
        }
        return result;
    }
};
```

# Vysvetlenie

Táto trieda je obdoba štandardnej FIFO fronty – teda **First In, First Out**.

## Použité hlavičkové súbory

```cpp
#include <iostream>  // pre vstup/výstup (napr. std::cout)
#include <vector>    // pre std::vector
```

## Trieda `Queue`

Trieda slúži na uchovávanie a manipuláciu s frontou reťazcov.

### Atribút:

```cpp
std::vector<std::string> values;
```

- Uchováva hodnoty vo fronte ako dynamické pole (vektor).

## Metódy

### `Queue()` – Konštruktor

```cpp
Queue() = default;
```

- Používa predvolený konštruktor – žiadne špeciálne správanie.

### `~Queue()` – Destruktor

```cpp
~Queue() = default;
```

- Opäť predvolený, lebo `std::vector` sa postará o uvoľnenie pamäte.

### `void push(const std::string &value)`

```cpp
values.push_back(value);
```

- Pridá nový prvok **na koniec** fronty.

### `std::string pop()`

```cpp
std::string front = values.front();
values.erase(values.begin());
return front;
```

- Vráti a odstráni prvok **z prednej časti** fronty.
- Toto je klasické FIFO správanie.
- ⚠️ Mínus: `erase(values.begin())` je **lineárne pomalé O(n)**, pretože sa musia prvky presúvať.

### `size_t size() const`

```cpp
return values.size();
```

- Vráti aktuálny počet prvkov vo fronte.

### `std::string front()`

```cpp
return values.front();
```

- Vráti (bez odstránenia) prvý prvok vo fronte.

### `std::string back()`

```cpp
return values.back();
```

- Vráti posledný prvok vo fronte (ten, ktorý bol pridaný ako posledný).

### `std::string to_string()`

```cpp
std::string result;
for (const std::string &value: values) {
    result += value + " ";
}
return result;
```

- Vytvorí reťazec reprezentujúci obsah fronty (hodnoty oddelené medzerou).

## Použitie (príklad)

```cpp
Queue q;
q.push("A");
q.push("B");
q.push("C");

std::cout << q.to_string(); // "A B C "

std::cout << q.pop(); // "A"
std::cout << q.front(); // "B"
```
