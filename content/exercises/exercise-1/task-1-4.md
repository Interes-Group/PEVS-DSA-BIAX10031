---
date: '2025-02-13T19:11:45+01:00'
title: 'Úloha 1.4'
weight: 4
---

Napíšte program v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta reťazec zo štandardného vstupu a zistí, či je tento reťazec palindróm (t.j. číta sa rovnako spredu aj
zozadu). Implementujte funkciu `bool je_palindrom(const std::string& text)`, ktorá vráti hodnotu `true`, ak je reťazec
palindróm, inak vráti `false`. V programe použite vhodné konštanty tam, kde je to potrebné.

### Príklady vstupov / výstupov programu

#### Príklad 1

- Vstup: `radar`
- Výstup: `Reťazec 'radar' je palindróm.`

#### Príklad 2

- Vstup: `programovanie`
- Výstup: `Reťazec 'programovanie' nie je palindróm.`

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}
Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

<!--
```cpp
#include <iostream>

bool je_palindrom(const char* text) {
    int dlzka = 0;
    while (text[dlzka] != '\0') {
        ++dlzka;
    }

    for (int i = 0; i < dlzka / 2; ++i) {
        if (text[i] != text[dlzka - 1 - i]) {
            return false;
        }
    }
    return true;
}

int main() {
    const int MAX_DLZKA = 100;
    char text[MAX_DLZKA];

    std::cout << "Zadajte reťazec: ";
    std::cin.getline(text, MAX_DLZKA);

    if (je_palindrom(text)) {
        std::cout << "Reťazec '" << text << "' je palindróm." << std::endl;
    } else {
        std::cout << "Reťazec '" << text << "' nie je palindróm." << std::endl;
    }

    return 0;
}
```

### Vysvetlenie

**Funkcia `je_palindrom`:**

- Funkcia prijíma ukazovateľ na znakové pole (`const char* text`).
- Výpočet dĺžky reťazca: Pomocou cyklu `while` sa zistí dĺžka reťazca tým, že sa iteruje až po nulový znak (`'\0'`).
- Kontrola palindrómu: V cykle `for` sa porovnávajú znaky od začiatku a konca reťazca smerom k stredu. Ak sa nájde
  dvojica
  znakov, ktoré sa nerovnajú, funkcia vráti `false`. Ak sú všetky takéto dvojice rovnaké, funkcia vráti `true`.

**Funkcia `main`:**

- Definícia konštanty: `MAX_DLZKA` je konštanta určujúca maximálnu dĺžku reťazca, ktorú program dokáže spracovať.
- Načítanie reťazca: Používa sa funkcia `std::cin.getline` na načítanie celého riadku textu vrátane medzier.
- Volanie funkcie `je_palindrom`: Na základe návratovej hodnoty tejto funkcie program vypíše, či je zadaný reťazec
  palindróm alebo nie.
-->
{{< /details >}}
<!--
{{< details title="Alternatívne, viac c++ riešenie" closed="true" >}}

```cpp
#include <iostream>
#include <string>
#include <algorithm>

bool je_palindrom(const std::string& text) {
    std::string reversed_text = text;
    std::reverse(reversed_text.begin(), reversed_text.end());
    return text == reversed_text;
}

int main() {
    std::string text;
    std::cout << "Zadajte reťazec: ";
    std::getline(std::cin, text);

    if (je_palindrom(text)) {
        std::cout << "Reťazec '" << text << "' je palindróm." << std::endl;
    } else {
        std::cout << "Reťazec '" << text << "' nie je palindróm." << std::endl;
    }

    return 0;
}
```

### Vysvetlenie

**Funkcia `je_palindrom`:**

- Funkcia prijíma reťazec `text` typu `std::string`.
- Vytvorí kópiu tohto reťazca s názvom `reversed_text`.
- Pomocou funkcie `std::reverse` z knižnice `<algorithm>` obráti obsah reťazca `reversed_text`.
- Porovná pôvodný reťazec `text` s obráteným reťazcom `reversed_text`. Ak sú rovnaké, funkcia vráti `true`, inak vráti
  `false`.

**Funkcia `main`:**

- Načíta reťazec od používateľa pomocou `std::getline`, čo umožňuje načítať celý riadok vrátane medzier.
- Volá funkciu `je_palindrom` a na základe jej návratovej hodnoty vypíše, či je zadaný reťazec palindróm alebo nie.

Tento program rozlišuje veľké a malé písmená a berie do úvahy aj medzery a interpunkčné znamienka. Ak chcete, aby
program ignoroval veľkosť písmen a nealfanumerické znaky, môžete pred porovnávaním reťazec pretransformovať na malé
písmená a odstrániť nealfanumerické znaky.

{{< /details >}}
-->