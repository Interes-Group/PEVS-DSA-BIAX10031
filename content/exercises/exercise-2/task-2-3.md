---
date: '2025-02-14T18:11:26+01:00'
title: 'Úloha 2.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte triedu `Student` so súkromnými atribútmi:

- `std::string meno`
- `int rocnik`
- `std::vector<double> znamky`

**Implementujte verejné metódy:**

- Konstruktor na inicializáciu _mena_ a _ročníka_.
- Metódu `pridaj_znamku(double znamka)`, ktorá pridá známku do zoznamu známok.
- Metódu `vypocitaj_priemer()`, ktorá vráti priemernú hodnotu známok.
- Metódu `vypis_info()`, ktorá vypíše informácie o študentovi v nasledujúcom formáte:

```text
Meno: <meno>
Ročník: <rocnik>
Priemerná známka: <priemer>
```

Použite triedu `std::vector` na ukladanie známok. Zabezpečte, aby meno a ročník nemohli byť priamo modifikované mimo
triedy. Ošetrite situácie, keď študent nemá žiadne známky pri výpočte priemeru. Pre demonštráciu implementácie vytvorte
vo funkcii `main` aspoň tri objekty a zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <numeric> // Pre std::accumulate

class Student {
private:
    std::string meno;
    int rocnik;
    std::vector<double> znamky;

public:
    // Konštruktor na inicializáciu mena a ročníka
    Student(const std::string& m, int r) : meno(m), rocnik(r) {}

    // Metóda na pridanie známky
    void pridaj_znamku(double znamka) {
        if (znamka >= 1.0 && znamka <= 5.0) {
            znamky.push_back(znamka);
        } else {
            std::cout << "Neplatná známka. Povolené hodnoty sú od 1.0 do 5.0.\n";
        }
    }

    // Metóda na výpočet priemernej známky
    double vypocitaj_priemer() const {
        if (znamky.empty()) {
            return 0.0; // Ak nie sú známky, vracia 0
        }
        return std::accumulate(znamky.begin(), znamky.end(), 0.0) / znamky.size();
    }

    // Metóda na výpis informácií o študentovi
    void vypis_info() const {
        std::cout << "Meno: " << meno << "\n"
                  << "Ročník: " << rocnik << "\n"
                  << "Priemerná známka: " << (znamky.empty() ? "N/A" : std::to_string(vypocitaj_priemer())) 
                  << "\n----------------------\n";
    }
};

int main() {
    // Vytvorenie objektov študentov
    Student student1("Ján Novák", 2);
    Student student2("Mária Horváthová", 3);
    Student student3("Peter Kováč", 1);

    // Pridanie známok
    student1.pridaj_znamku(1.5);
    student1.pridaj_znamku(2.0);
    student1.pridaj_znamku(1.0);

    student2.pridaj_znamku(3.0);
    student2.pridaj_znamku(2.5);
    student2.pridaj_znamku(4.0);

    student3.pridaj_znamku(5.0);
    student3.pridaj_znamku(4.5);

    // Výpis informácií o študentoch
    student1.vypis_info();
    student2.vypis_info();
    student3.vypis_info();

    return 0;
}
```

### Vysvetlenie:
1. **Zapuzdrenie:**
    - Atribúty `meno`, `rocnik` a `znamky` sú súkromné (`private`), aby neboli priamo modifikovateľné mimo triedy.

2. **Konštruktor:**
    - Inicializuje meno a ročník študenta.

3. **Metódy:**
    - `pridaj_znamku(double znamka)`: Pridá známku do vektora `znamky`, ak je v rozsahu `1.0 - 5.0`.
    - `vypocitaj_priemer()`: Počíta aritmetický priemer známok, pričom kontroluje, či zoznam nie je prázdny.
    - `vypis_info()`: Vypíše meno, ročník a priemernú známku (ak neexistujú známky, zobrazí "N/A").

4. **Použitie `std::vector<double>`:**
    - `znamky` sú uložené v dynamickom kontajneri `std::vector`, čo umožňuje efektívne pridávanie nových hodnôt.

5. **Ošetrenie prázdneho zoznamu známok:**
    - Ak študent nemá žiadne známky, priemer sa vráti ako `0.0` a vo výpise sa zobrazí `"N/A"`.

### Príklad výstupu:
```
Meno: Ján Novák
Ročník: 2
Priemerná známka: 1.5
----------------------
Meno: Mária Horváthová
Ročník: 3
Priemerná známka: 3.166667
----------------------
Meno: Peter Kováč
Ročník: 1
Priemerná známka: 4.75
----------------------
```

{{< /details >}}
