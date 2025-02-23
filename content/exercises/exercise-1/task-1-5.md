---
date: '2025-02-13T19:11:47+01:00'

title: 'Úloha 1.5'
weight: 5
---

Napíšte program v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Program načíta dva reťazce zo štandardného vstupu a zistí, či sú tieto reťazce anagramy (t.j. obsahujú rovnaké znaky v
rovnakom počte, ale v rôznom poradí). Implementujte funkciu
`bool su_anagramy(const std::string& prvy, const std::string& druhy)`, ktorá vráti hodnotu `true`, ak sú reťazce
anagramy, inak vráti `false`. V programe použite vhodné konštanty tam, kde je to potrebné.

### Príklady vstupov / výstupov programu

#### Príklad 1

- Vstup: `listen silent`
- Výstup: `Reťazce 'listen' a 'silent' sú anagramy.`

#### Príklad 2

- Vstup: `hello world`
- Výstup: `Reťazce 'hello' a 'world' nie sú anagramy.`

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <string>
#include <algorithm>

bool su_anagramy(const std::string& prvy, const std::string& druhy) {
    if (prvy.length() != druhy.length()) {
        return false;
    }

    std::string prvy_triedeny = prvy;
    std::string druhy_triedeny = druhy;

    std::sort(prvy_triedeny.begin(), prvy_triedeny.end());
    std::sort(druhy_triedeny.begin(), druhy_triedeny.end());

    return prvy_triedeny == druhy_triedeny;
}

int main() {
    std::string prvy_retazec, druhy_retazec;

    std::cout << "Zadajte prvý reťazec: ";
    std::getline(std::cin, prvy_retazec);

    std::cout << "Zadajte druhý reťazec: ";
    std::getline(std::cin, druhy_retazec);

    if (su_anagramy(prvy_retazec, druhy_retazec)) {
        std::cout << "Reťazce '" << prvy_retazec << "' a '" << druhy_retazec << "' sú anagramy." << std::endl;
    } else {
        std::cout << "Reťazce '" << prvy_retazec << "' a '" << druhy_retazec << "' nie sú anagramy." << std::endl;
    }

    return 0;
}
```

### Vysvetlenie

**Funkcia `su_anagramy`:**

- Funkcia prijíma dva reťazce `prvy` a `druhy` typu `std::string`.
- Najprv skontroluje, či majú oba reťazce rovnakú dĺžku. Ak nie, vráti `false`, pretože reťazce rôznej dĺžky nemôžu byť
  anagramy.
- Vytvorí kópie oboch reťazcov s názvami `prvy_triedeny` a `druhy_triedeny`.
- Pomocou funkcie `std::sort` z knižnice `<algorithm>` zotriedi znaky v oboch reťazcoch.
- Porovná zotriedené reťazce; ak sú rovnaké, vráti `true`, inak vráti `false`.

**Funkcia `main`:**

- Načíta dva reťazce od používateľa pomocou `std::getline`, čo umožňuje načítať celé riadky vrátane medzier.
- Volá funkciu `su_anagramy` a na základe jej návratovej hodnoty vypíše, či sú zadané reťazce anagramy alebo nie.

{{< /details >}}
