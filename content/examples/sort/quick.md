---
date: '2025-03-24T11:22:07+01:00'
title: 'Quicksort'
---

**Quicksort** – efektívny a veľmi populárny algoritmus na triedenie poľa, ktorý funguje na princípe **rozdeľ a panuj (
divide and conquer)**.

###  Princíp:

1. **Vyberie sa pivot** (napr. prvý alebo posledný prvok).
2. **Rozdelí sa pole** na dve časti:
    - Prvky menšie alebo rovné pivotu (vľavo)
    - Prvky väčšie ako pivot (vpravo)
3. **Rekurzívne sa triedia obe časti.**

###  Časová zložitosť:

- **Priemerne:** O(n log n)
- **V najhoršom prípade (už zoradené pole):** O(n²)
- **Pamäťová zložitosť:** O(log n) kvôli rekurzii

###  Výhody:

- Veľmi rýchly v praxi (aj v porovnaní s Merge sortom)
- Triedi „na mieste“ (nepotrebuje ďalšiu pamäť)

### Nevýhody:

- Nie je stabilný (nezachováva poradie rovnakých prvkov)
- Najhorší prípad má zložitosť O(n²)

## Implementácia

```cpp
#include <iostream>
#include <vector>

using namespace std;

void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

/**
* Implementácia výberu pivota podľa Hoare schémy.
* Táto funkcia berie prvý prvok ako pivot a umiestňuje
   všetky prvky menšie ako pivot na ľavej strane
   a všetky prvky väčšie ako pivot na
   na pravej strane. Vráti index posledného prvku
   na menšej strane.
 * @param values hodnoty pola
 * @param low dolny index
 * @param high horny index
 * @return poziciu pivota
 */
int hoare_partition(vector<int> &values, int low, int high) {
    int pivot = values[low];
    int i = low;
    int j = high;

    while (true) {
        while (values[i] < pivot) {
            i++;
        }
        while (values[j] > pivot) {
            j--;
        }
        if (i >= j) return j;
        swap(values[i], values[j]);
    }
}

/**
* Implementácia algoritmu Lomuto pre výber pivota.
* Táto funkcia berie posledný prvok ako pivot, umiestni
   otočný prvok na jeho správnu pozíciu v zoradenom
   a umiestni všetky menšie (menšie ako pivot)
   naľavo od pivotu a všetky väčšie prvky
   napravo od pivotu
 * @param values hodnotyp pola
 * @param low dolny index
 * @param high horny index
 * @return poziciu pivota
 */
int lomuto_partition(vector<int> &values, int low, int high) {
    int pivot = values[high]; // vyber ako pivota prvok najviac v pravo
    int i = low; // ukazovatel prehodenia prvku

    // prechadzanie prvkov a porovnanie s pivotom
    for (int j = low; j < high; j++) {
        if (values[j] <= pivot) {
            // tu je porovnanie
            // kedze prvok je mensi ako pivot, prehod ho s tim co je na ukazovateli
            swap(values[i], values[j]);
            i++; // posunieme ukazovatel
        }
    }
    swap(values[i], values[high]);
    return i;
}

int partition(vector<int> &values, int low, int high, int algorithm) {
    if(algorithm >= 1)
        return lomuto_partition(values, low, high);
    else
        return hoare_partition(values, low, high);
}

void quicksort(vector<int> &values, int low, int high, int algorithm) {
    if (low < high) {
        // najdi pivot taky, kde vsetky mensie prvky su vlavo a vacsie prvky su vpravo
        int pivot = partition(values, low, high, algorithm);

        quicksort(values, low, pivot - 1);
        quicksort(values, pivot + 1, high);
    }
}

void print(const vector<int> &vs) {
    for (int v: vs) {
        cout << v << " ";
    }
    cout << endl;
}

int main() {
    vector<int> v = {11, -2, 4, 52, -6, 17, 28};
    print(v);
    quicksort(v, 0, v.size() - 1, 1);
    print(v);
    return 0;
}
```

## Vysvetlenie jednotlivých častí kódu

### `swap(int &a, int &b)`

Jednoduchá pomocná funkcia na **prehodenie hodnôt dvoch premenných** cez referenciu.

### `hoare_partition(...)`

```cpp
int hoare_partition(vector<int> &values, int low, int high)
```

- Používa **Hoareho schému** výberu pivota (pivot = prvý prvok).
- Premenné `i` a `j` sa rozbiehajú z oboch strán poľa a hľadajú dvojice prvkov, ktoré sú v zlom poradí.
- Po nájdení takýchto dvojíc sa prehodia.
- Ukončí sa, keď sa `i` a `j` stretnú alebo minú.

➡️ Vracia **index, ktorý rozdeľuje pole** na dve časti:

- ľavá časť ≤ pivot
- pravá časť ≥ pivot

### `lomuto_partition(...)`

```cpp
int lomuto_partition(vector<int> &values, int low, int high)
```

- Používa **Lomuto schému** výberu pivota (pivot = posledný prvok).
- Prechádza celé pole a udržiava index `i`, kam sa vkladajú prvky menšie alebo rovné pivotu.
- Po prejdení poľa sa pivot umiestni za posledný menší prvok (na pozíciu `i`).

➡️ Vracia index nového umiestnenia pivota.

### `partition(...)`

```cpp
int partition(vector<int> &values, int low, int high, int algorithm)
```

- Vyberá, ktorú partition stratégiu použiť (Lomuto alebo Hoare) podľa parametra `algorithm`.

### `quicksort(...)`

```cpp
void quicksort(vector<int> &values, int low, int high, int algorithm)
```

- Rekurzívna implementácia **Quicksortu**.
- Vyberie pivot (pomocou `partition()`), a potom rekurzívne triedi:
    - ľavú časť (`low` až `pivot - 1`)
    - pravú časť (`pivot + 1` až `high`)

### `main()` a `print(...)`

- `main()` inicializuje vektor čísel, vypíše ho, spustí triedenie a znova vypíše výsledok.
- Triedi sa pomocou **Lomuto partition schémy** (pretože `algorithm = 1`).
