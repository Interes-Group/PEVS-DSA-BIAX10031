---
date: '2025-02-14T18:11:24+01:00'
title: 'Úloha 2.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte triedu `BankovyUcet` so súkromnými atribútmi:

- `std::string cislo_uctu`
- `double zostatok`

**Implementujte verejné metódy:**

- Konstruktor na inicializáciu _čísla účtu_ a _počiatočného zostatku_.
- Metódu `vloz(double suma)`, ktorá zvýši zostatok o zadanú sumu.
- Metódu `vyber(double suma)`, ktorá zníži zostatok o zadanú sumu, ak je dostatok prostriedkov; inak vypíše varovanie o
  nedostatočnom zostatku.
- Metódu `ziskaj_zostatok()`, ktorá vráti aktuálny zostatok.

Zabezpečte, aby zostatok nemohol byť priamo modifikovaný mimo triedy. Ošetrite situácie, keď sa pokúsite vybrať viac
prostriedkov, než je dostupné na účte. Pre demonštráciu implementácie vytvorte vo funkcii `main` aspoň tri objekty a
zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <string>

class BankovyUcet {
private:
    std::string cislo_uctu;
    double zostatok;

public:
    // Konštruktor inicializuje číslo účtu a počiatočný zostatok
    BankovyUcet(const std::string& cislo, double pociatocny_zostatok)
        : cislo_uctu(cislo), zostatok(pociatocny_zostatok) {}

    // Metóda na vklad peňazí
    void vloz(double suma) {
        if (suma > 0) {
            zostatok += suma;
            std::cout << "Na účet " << cislo_uctu << " bolo vložené: " << suma << " EUR.\n";
        } else {
            std::cout << "Neplatná suma na vklad.\n";
        }
    }

    // Metóda na výber peňazí
    void vyber(double suma) {
        if (suma > 0 && suma <= zostatok) {
            zostatok -= suma;
            std::cout << "Z účtu " << cislo_uctu << " bolo vybrané: " << suma << " EUR.\n";
        } else {
            std::cout << "Nedostatok prostriedkov na účte " << cislo_uctu << "!\n";
        }
    }

    // Metóda na získanie aktuálneho zostatku
    double ziskaj_zostatok() const {
        return zostatok;
    }

    // Výpis informácií o účte
    void vypis_info() const {
        std::cout << "Účet: " << cislo_uctu << "\n"
                  << "Zostatok: " << zostatok << " EUR\n"
                  << "----------------------\n";
    }
};

int main() {
    // Vytvorenie troch objektov triedy BankovyUcet
    BankovyUcet ucet1("SK1000000001", 500.0);
    BankovyUcet ucet2("SK1000000002", 1000.0);
    BankovyUcet ucet3("SK1000000003", 250.0);

    // Použitie metód na vklad, výber a výpis informácií
    ucet1.vypis_info();
    ucet1.vloz(200);
    ucet1.vyber(100);
    ucet1.vypis_info();

    ucet2.vypis_info();
    ucet2.vyber(1200); // Očakáva sa varovanie o nedostatku prostriedkov
    ucet2.vyber(500);
    ucet2.vypis_info();

    ucet3.vypis_info();
    ucet3.vloz(300);
    ucet3.vyber(50);
    ucet3.vypis_info();

    return 0;
}
```

### Vysvetlenie:
1. **Zapuzdrenie**
  - Atribúty `cislo_uctu` a `zostatok` sú súkromné (`private`), takže ich nemožno meniť priamo, ale iba cez metódy triedy.

2. **Konštruktor**
  - Inicializuje číslo účtu a počiatočný zostatok.

3. **Metódy:**
  - `vloz(double suma)`: Zvyšuje zostatok, ale len ak je suma kladná.
  - `vyber(double suma)`: Zníži zostatok, ale iba ak je suma kladná a je na účte dostatok prostriedkov.
  - `ziskaj_zostatok()`: Vráti aktuálny zostatok.
  - `vypis_info()`: Vypíše informácie o účte.

4. **Ochrana pred neplatnými operáciami**
  - Pri pokuse o výber viac peňazí, než je na účte, sa zobrazí varovanie.

### Príklad výstupu:
```
Účet: SK1000000001
Zostatok: 500 EUR
----------------------
Na účet SK1000000001 bolo vložené: 200 EUR.
Z účtu SK1000000001 bolo vybrané: 100 EUR.
Účet: SK1000000001
Zostatok: 600 EUR
----------------------
Účet: SK1000000002
Zostatok: 1000 EUR
----------------------
Nedostatok prostriedkov na účte SK1000000002!
Z účtu SK1000000002 bolo vybrané: 500 EUR.
Účet: SK1000000002
Zostatok: 500 EUR
----------------------
Účet: SK1000000003
Zostatok: 250 EUR
----------------------
Na účet SK1000000003 bolo vložené: 300 EUR.
Z účtu SK1000000003 bolo vybrané: 50 EUR.
Účet: SK1000000003
Zostatok: 500 EUR
----------------------
```

{{< /details >}}
