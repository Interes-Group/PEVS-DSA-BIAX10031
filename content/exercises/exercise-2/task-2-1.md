---
date: '2025-02-14T18:09:57+01:00'
title: 'Úloha 2.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte triedu `Kniha`, ktorá bude mať súkromné atribúty:

- `std::string nazov`
- `std::string autor`
- `std::string isbn`

Implementujte verejné metódy na nastavenie a získanie hodnôt týchto atribútov. Ďalej pridajte metódu `vypis_info()`,
ktorá
vypíše informácie o knihe v nasledujúcom formáte:

```text
Názov: <nazov>
Autor: <autor>
ISBN: <isbn>
```

Pri implementácií triedy použitie zapúzdrenie členov/atribútov triedy. Zabezpečte, aby všetky reťazce boli spracované
pomocou triedy `std::string`. Pre demonštráciu implementácie vytvorte vo funkcii `main` aspoň tri objekty a zavolajte na
nich metódu `vypis_info()`.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}
Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

<!--
```cpp
#include <iostream>
#include <string>

class Kniha {
private:
    std::string nazov;
    std::string autor;
    std::string isbn;

public:
    // Konštruktory
    Kniha() = default; // Defaultný konštruktor

    Kniha(const std::string& n, const std::string& a, const std::string& i)
        : nazov(n), autor(a), isbn(i) {}

    // Gettery a Settery
    void setNazov(const std::string& n) { nazov = n; }
    void setAutor(const std::string& a) { autor = a; }
    void setIsbn(const std::string& i) { isbn = i; }

    std::string getNazov() const { return nazov; }
    std::string getAutor() const { return autor; }
    std::string getIsbn() const { return isbn; }

    // Metóda na výpis informácií o knihe
    void vypis_info() const {
        std::cout << "Názov: " << nazov << "\n"
                  << "Autor: " << autor << "\n"
                  << "ISBN: " << isbn << "\n"
                  << "------------------" << std::endl;
    }
};

int main() {
    // Vytvorenie objektov triedy Kniha
    Kniha kniha1("Meno vetra", "Patrick Rothfuss", "978-80-7388-739-9");
    Kniha kniha2("Hobit", "J.R.R. Tolkien", "978-80-551-2060-5");
    Kniha kniha3("1984", "George Orwell", "978-80-7106-371-9");

    // Výpis informácií o knihách
    kniha1.vypis_info();
    kniha2.vypis_info();
    kniha3.vypis_info();

    return 0;
}
```

### Vysvetlenie:
1. **Zapuzdrenie** – Atribúty `nazov`, `autor` a `isbn` sú súkromné (`private`), takže ich nemožno meniť priamo, ale iba cez metódy `setNazov()`, `setAutor()`, `setIsbn()`.
2. **Použitie `std::string`** – Na manipuláciu s textovými údajmi.
3. **Gettery a settery** – Na získavanie a nastavovanie hodnôt atribútov.
4. **Metóda `vypis_info()`** – Formátovaný výpis informácií o knihe.
5. **Vytvorenie a demonštrácia objektov** – Vo funkcii `main` sa vytvoria tri objekty triedy `Kniha` a vypíšu sa ich informácie.

### Príklad výstupu:
```
Názov: Meno vetra
Autor: Patrick Rothfuss
ISBN: 978-80-7388-739-9
------------------
Názov: Hobit
Autor: J.R.R. Tolkien
ISBN: 978-80-551-2060-5
------------------
Názov: 1984
Autor: George Orwell
ISBN: 978-80-7106-371-9
------------------
```
-->
{{< /details >}}
