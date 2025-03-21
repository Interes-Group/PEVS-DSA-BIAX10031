---
date: '2025-03-21T23:47:16+01:00'
title: 'Stack'
---

Príklad implementácie zásobniku reťazcov. Úložisko prvkov zásobníka je implementovaný pomocou zoznamu (list) nakoľko je
efektívnejší na pridanie na začiatok.

```cpp
#include <string>
#include <list>

class Stack {
private:
    std::list<std::string> values;

public:
    Stack() = default;

    ~Stack() = default;

    void push(std::string value) {
        values.insert(values.begin(), value);
    }

    std::string pop() {
        std::string front = values.front();
        values.pop_front();
        return front;
    }

    std::string top() {
        return values.front();
    }

    size_t size() { return values.size(); }

    std::string to_string() {
        std::string result;
        for (auto it = values.begin(); it != values.end(); it++) {
            result += *it + " ";
        }
        return result;
    }
};
```

# Vysvetlenie

Ide o **LIFO (Last In, First Out)** štruktúru – posledný pridaný prvok sa vyberá ako prvý.

## Použité knižnice

```cpp
#include <string>  // pre prácu s reťazcami (std::string)
#include <list>    // pre obojsmerný zoznam (std::list)
```

## Trieda `Stack`

Táto trieda predstavuje zásobník reťazcov.

### Atribúty (private):

```cpp
std::list<std::string> values;
```

- Interná dátová štruktúra na ukladanie hodnôt.
- `std::list` je **dvojito viazaný zoznam**, vďaka čomu je vkladanie a odstraňovanie na začiatku O(1) – ideálne pre
  zásobník.

## Metódy triedy

### `Stack()` a `~Stack()`

```cpp
Stack() = default;
~Stack() = default;
```

- Používajú **predvolené** konštruktory a destruktory (žiadna špeciálna inicializácia alebo čistenie nie je potrebné).

### `void push(std::string value)`

```cpp
values.insert(values.begin(), value);
```

- Vloží nový prvok **na začiatok zoznamu** (vrchol zásobníka).
- Rýchla operácia v `std::list` (O(1)).

### `std::string pop()`

```cpp
std::string front = values.front();
values.pop_front();
return front;
```

- Zoberie prvý prvok (vrchol zásobníka), odstráni ho a vráti.
- LIFO logika: posledný pridaný prvok ide ako prvý von.

### `std::string top()`

```cpp
return values.front();
```

- Vráti prvok na vrchole zásobníka **bez jeho odstránenia**.

### `size_t size()`

```cpp
return values.size();
```

- Vráti počet prvkov v zásobníku.

### `std::string to_string()`

```cpp
std::string result;
for (auto it = values.begin(); it != values.end(); it++) {
    result += *it + " ";
}
return result;
```

- Prejde všetky hodnoty v zásobníku (zhora nadol) a spojí ich do jedného reťazca oddeleného medzerou.
- Môže slúžiť na výpis obsahu zásobníka.

## Použitie (príklad)

```cpp
Stack s;
s.push("A");
s.push("B");
s.push("C");

std::cout << s.to_string(); // "C B A "
std::cout << s.top();       // "C"
std::cout << s.pop();       // "C"
std::cout << s.top();       // "B"
```

## Prečo `std::list`?

- `std::list` umožňuje efektívne vkladanie/odoberanie na začiatku (O(1)).
- `std::vector` by mohol byť pomalší pri vkladaní na začiatok (O(n)).
- Alebo možno použiť `std::stack` z `<stack>` ak netreba vlastnú implementáciu.
