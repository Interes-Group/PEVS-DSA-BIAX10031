---
date: '2025-02-14T18:11:27+01:00'
title: 'Úloha 2.4'
weight: 4
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte systém pre správu objednávok v obchode. Systém by mal obsahovať triedy `Produkt`, `Objednavka` a `Zakaznik`. 
Trieda `Zakaznik` by mala mať možnosť pridávať objednávky a trieda `Objednavka` by mala obsahovať zoznam produktov.

**Trieda `Produkt`:**

- _Atribúty_:
    - `std::string nazov`
    - `double cena`
- _Metódy_:
    - Konstruktor na inicializáciu názvu a ceny produktu.
    - Metódy na získanie názvu a ceny produktu.

**Trieda `Objednavka`:**

- Atribúty:
    - `std::vector<Produkt> produkty`
- Metódy:
    - Metóda na pridanie produktu do objednávky.
    - Metóda na výpočet celkovej ceny objednávky.

**Trieda `Zakaznik`:**

- Atribúty:
    - `std::string meno`
    - `std::vector<Objednavka> objednavky`
- Metódy:
    - Konstruktor na inicializáciu mena zákazníka.
    - Metóda na pridanie novej objednávky.
    - Metóda na zobrazenie všetkých objednávok zákazníka vrátane celkovej ceny každej objednávky.

Využite triedu `std::vector` na ukladanie zoznamov produktov a objednávok. Dbajte na správne zapuzdrenie údajov a
poskytnite potrebné metódy na prístup k súkromným atribútom. Pri implementácii metódy na výpočet celkovej ceny
objednávky iterujte cez zoznam produktov a sčítajte ich ceny. Pri zobrazení objednávok zákazníka vypíšte názvy produktov
v objednávke a celkovú cenu objednávky. Pre demonštráciu implementácie vytvorte vo funkcii `main` aspoň tri objekty a
zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <vector>
#include <string>

class Produkt {
private:
    std::string nazov;
    double cena;

public:
    // Konštruktor na inicializáciu názvu a ceny produktu
    Produkt(const std::string& n, double c) : nazov(n), cena(c) {}

    // Gettery
    std::string getNazov() const { return nazov; }
    double getCena() const { return cena; }
};

class Objednavka {
private:
    std::vector<Produkt> produkty;

public:
    // Metóda na pridanie produktu do objednávky
    void pridajProdukt(const Produkt& produkt) {
        produkty.push_back(produkt);
    }

    // Metóda na výpočet celkovej ceny objednávky
    double vypocitajCenu() const {
        double celkovaCena = 0;
        for (const auto& produkt : produkty) {
            celkovaCena += produkt.getCena();
        }
        return celkovaCena;
    }

    // Výpis obsahu objednávky
    void vypisObjednavku() const {
        std::cout << "Objednávka obsahuje:" << std::endl;
        for (const auto& produkt : produkty) {
            std::cout << "- " << produkt.getNazov() << " (" << produkt.getCena() << " EUR)" << std::endl;
        }
        std::cout << "Celková cena: " << vypocitajCenu() << " EUR" << std::endl;
        std::cout << "----------------------\n";
    }
};

class Zakaznik {
private:
    std::string meno;
    std::vector<Objednavka> objednavky;

public:
    // Konštruktor na inicializáciu mena zákazníka
    Zakaznik(const std::string& m) : meno(m) {}

    // Metóda na pridanie novej objednávky
    void pridajObjednavku(const Objednavka& objednavka) {
        objednavky.push_back(objednavka);
    }

    // Metóda na zobrazenie všetkých objednávok zákazníka
    void vypisObjednavky() const {
        std::cout << "Zákazník: " << meno << std::endl;
        if (objednavky.empty()) {
            std::cout << "Zákazník zatiaľ nemá žiadne objednávky.\n";
        } else {
            for (size_t i = 0; i < objednavky.size(); ++i) {
                std::cout << "Objednávka #" << i + 1 << ":\n";
                objednavky[i].vypisObjednavku();
            }
        }
        std::cout << "======================\n";
    }
};

int main() {
    // Vytvorenie produktov
    Produkt produkt1("Notebook", 1200.50);
    Produkt produkt2("Smartphone", 799.99);
    Produkt produkt3("Slúchadlá", 150.75);
    Produkt produkt4("Monitor", 299.99);
    Produkt produkt5("Klávesnica", 49.99);

    // Vytvorenie objednávok a pridanie produktov
    Objednavka objednavka1;
    objednavka1.pridajProdukt(produkt1);
    objednavka1.pridajProdukt(produkt3);

    Objednavka objednavka2;
    objednavka2.pridajProdukt(produkt2);
    objednavka2.pridajProdukt(produkt4);
    objednavka2.pridajProdukt(produkt5);

    // Vytvorenie zákazníkov a pridanie objednávok
    Zakaznik zakaznik1("Peter Novák");
    zakaznik1.pridajObjednavku(objednavka1);

    Zakaznik zakaznik2("Jana Horváthová");
    zakaznik2.pridajObjednavku(objednavka2);

    // Výpis objednávok zákazníkov
    zakaznik1.vypisObjednavky();
    zakaznik2.vypisObjednavky();

    return 0;
}
```

### **Vysvetlenie:**
1. **Trieda `Produkt`**
  - Obsahuje názov a cenu produktu.
  - Má metódy na získanie názvu a ceny.

2. **Trieda `Objednavka`**
  - Obsahuje zoznam produktov.
  - Má metódu na pridanie produktu do objednávky.
  - Metóda `vypocitajCenu()` spočíta celkovú cenu všetkých produktov v objednávke.
  - Metóda `vypisObjednavku()` zobrazí zoznam produktov a ich ceny.

3. **Trieda `Zakaznik`**
  - Obsahuje meno zákazníka a jeho objednávky.
  - Metóda `pridajObjednavku()` umožňuje pridať novú objednávku.
  - Metóda `vypisObjednavky()` zobrazí všetky objednávky zákazníka spolu s ich obsahom a cenou.

4. **Použitie `std::vector`**
  - Produkty sú uložené v dynamickom zozname objednávky.
  - Objednávky zákazníka sú uložené v dynamickom zozname.

### **Príklad výstupu programu:**

```
Zákazník: Peter Novák
Objednávka #1:
Objednávka obsahuje:
- Notebook (1200.5 EUR)
- Slúchadlá (150.75 EUR)
Celková cena: 1351.25 EUR
----------------------
======================
Zákazník: Jana Horváthová
Objednávka #1:
Objednávka obsahuje:
- Smartphone (799.99 EUR)
- Monitor (299.99 EUR)
- Klávesnica (49.99 EUR)
Celková cena: 1149.97 EUR
----------------------
======================
```

{{< /details >}}
