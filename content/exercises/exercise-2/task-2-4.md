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

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
