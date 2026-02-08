---
date: '2025-03-23T20:54:16+01:00'
title: 'Úloha 5.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte trieďací algoritmus **Bubble sort** na STL kontajnery `vector<string>`, t.j. vector reťazcov.
Pre porovnanie prvkov vektora môžte použiť funkciu [
`std::strcmp()`](https://en.cppreference.com/w/cpp/string/byte/strcmp).
Funkciu pre zotriedenie vektora implementujte tak aby bolo možné zadať ako parameter či má algoritmus zotriediť prvky
vzostupne (prvé sú reťazce čo lexikograficky skôr), zostupne (prvé sú reťazce, ktoré sú lexikograficky neskôr).

Viac o Bubble sort algoritme sa viete dozvedieť napríklad na stránkach:

- [https://en.wikipedia.org/wiki/Bubble_sort](https://en.wikipedia.org/wiki/Bubble_sort)
- [https://www.programiz.com/dsa/bubble-sort](https://www.programiz.com/dsa/bubble-sort)
- [https://www.geeksforgeeks.org/bubble-sort-algorithm/](https://www.geeksforgeeks.org/bubble-sort-algorithm/)

### Príklady vstupov / výstupov programu

```cpp
["Milan", "Martin", "Eva"] -sort-> ["Eva","Martin","Milan"]

["Fero", "Jano", ""]       -sort-> ["", "Fero", "Jano"]
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

<!--


```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cstring>  // pre std::strcmp

using namespace std;

/**
 * Bubble sort pre vector<string>, podporuje vzostupné aj zostupné poradie.
 *
 * @param arr       Vektor reťazcov, ktorý sa má zotriediť.
 * @param ascending True = vzostupne, False = zostupne.
 */
void bubbleSort(vector<string> &arr, bool ascending = true) {
    size_t n = arr.size();
    for (size_t i = 0; i < n; ++i) {
        // Po každom prejdení i prvkov na konci je už správne zoradených,
        // preto vnútorný cyklus ide len do n - i - 1.
        for (size_t j = 0; j + 1 < n - i; ++j) {
            int cmp = std::strcmp(arr[j].c_str(), arr[j + 1].c_str());
            // pre vzostupné poradie swapneme, ak je arr[j] > arr[j+1]
            // pre zostupné, ak je arr[j] < arr[j+1]
            if ((ascending  && cmp > 0) ||
                (!ascending && cmp < 0)) {
                std::swap(arr[j], arr[j + 1]);
            }
        }
    }
}

/**
 * Pomocná funkcia na výpis vektora reťazcov.
 */
void printVector(const vector<string> &v) {
    cout << "[ ";
    for (const auto &s : v) {
        cout << '"' << s << "\" ";
    }
    cout << "]\n";
}

int main() {
    // Príklad 1: vzostupné poradie
    vector<string> v1 = { "Milan", "Martin", "Eva" };
    cout << "Povodny vektor: ";
    printVector(v1);

    bubbleSort(v1, true);
    cout << "Zoradene vzostupne: ";
    printVector(v1);
    // Očakávaný výstup: ["Eva", "Martin", "Milan"]

    // Príklad 2: vzostupné (obsahuje prázdny reťazec)
    vector<string> v2 = { "Fero", "Jano", "" };
    cout << "\nPovodny vektor: ";
    printVector(v2);

    bubbleSort(v2, true);
    cout << "Zoradene vzostupne: ";
    printVector(v2);
    // Očakávaný výstup: ["", "Fero", "Jano"]

    // Príklad 3: zostupné poradie
    vector<string> v3 = { "Apple", "Banana", "Cherry" };
    cout << "\nPovodny vektor: ";
    printVector(v3);

    bubbleSort(v3, false);
    cout << "Zoradene zostupne: ";
    printVector(v3);
    // Očakávaný výstup: ["Cherry", "Banana", "Apple"]

    return 0;
}
```

### Vysvetlenie

- Funkcia `bubbleSort` prechádza vektor spustením dvoch vnútorných slučiek. Každým prechodom „buble“ najväčší (alebo
  najmenší) prvok na koniec nespracovanej časti vektora.
- Porovnanie reťazcov sa vykonáva pomocou `std::strcmp`, ktorý vracia:
    - záporné číslo, ak je prvý reťazec lexikograficky menší,
    - nulou, ak sú rovnaké,
    - kladné číslo, ak je väčší.
- Parameter `ascending` určuje, či sa swap uskutoční pre `cmp > 0` (vzostupne) alebo pre `cmp < 0` (zostupne).
- V `main` sú ukážky troch príkladov: dva pre vzostupné zotriedenie (jeden vrátane prázdneho reťazca) a jeden pre
  zostupné zotriedenie.

-->
{{< /details >}}
