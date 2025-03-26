---
date: '2025-03-24T11:22:01+01:00'
title: 'Merge Sort'
---

**Merge Sort** – efektívny triediaci algoritmus založený na princípe **rozdeľ a panuj (divide and conquer)**.

### Princíp:

1. **Rozdelenie**:
    - Rozdeľujeme pole na polovice, kým každá časť obsahuje iba 1 prvok.
    - Tieto časti sú triválne zoradené.

2. **Zlučovanie**:
    - Zoradené časti sa **spájajú** do väčších zoradených celkov.
    - Spájanie dvoch zoradených polí je rýchle (lineárne).

### Príklad:

Pole: `[11, -2, 4, 52, -6, 17, 28]`

Rozdelenie:

```
[11, -2, 4]   a   [52, -6, 17, 28]
→ [11] [-2, 4]   → [-2] [4]
→ zoradenie a zlučovanie: [-2, 4, 11]
```

Rovnaký proces pre druhú časť, potom sa zlúčia obe.

#### Časová zložitosť:

- **Najhorší / priemerný / najlepší prípad**: O(n log n)
- Efektívnejší ako Bubble sort a Selection sort (O(n²))

#### Pamäťová zložitosť:

- Vyžaduje **dodatočný priestor O(n)** (pomocné polia `left`, `right`)

### Výhody:

- Stabilný (zachová poradie rovnakých prvkov).
- Vhodný pre veľké a externé dátové sady.
- Predvídateľná zložitosť O(n log n).

### Nevýhody:

- Potrebuje dodatočnú pamäť.
- Mierne pomalší na malých dátach ako napr. Insertion sort.

## Implementácia

```cpp
#include <iostream>
#include <vector>

using namespace std;

void merge(vector<int>& values, int left_index, int middle_index, int right_index) {
    int left_size = middle_index - left_index + 1; // počet prvkov v ľavom podpoli
    int right_size = right_index - middle_index; // počet prvkov v pravom podpoli

    vector<int> left(left_size);
    vector<int> right(right_size);

    // prekopirovanie prvkov do laveho podpola
    for (int i = 0; i < left_size; i++) {
        left[i] = values[left_index + i];
    }

    // prekopirovanie prvkov do praveho podpola
    for (int j = 0; j < right_size; j++) {
        right[j] = values[middle_index + j + 1];
    }

    int i = 0, j = 0;
    int k = left_index;

    // kým neprídeme na koniec jedného z podpolí
    // daj do pola prvky z podpolia v správnom poradí
    while (i < left_size && j < right_size) {
        if (left[i] <= right[j]) { // podmienka zoradenia
            values[k] = left[i];
            i++;
        } else {
            values[k] = right[j];
            j++;
        }
        k++;
    }

    // kedze lava alebo prava strana môze byt vacsia musíme ešte dobehnúť jednotlivé polia pre oba prípady

    // dobehnutie laveho pola
    while (i < left_size) {
        values[k] = left[i];
        i++;
        k++;
    }
    // dobehnutie praveho pola
    while (j < right_size) {
        values[k] = right[j];
        j++;
        k++;
    }
}

void merge_sort(vector<int>& values, int l, int r) {
    if (l < r) {
        int mid = l + (r - l) / 2;
        merge_sort(values, l, mid);
        merge_sort(values, mid + 1, r);
        merge(values, l, mid, r);
    }
}

void print(const vector<int> &v) {
    for (int i: v) {
        cout << i << " ";
    }
    cout << endl;
}

int main() {
    vector<int> v = { 11, -2, 4, 52, -6, 17, 28};
    print(v);
    merge_sort(v, 0, v.size() - 1);
    print(v);
    return 0;
}
```

## Vysvetlenie častí kódu

#### `main()`

```cpp
vector<int> v = {11, -2, 4, 52, -6, 17, 28};
print(v);
merge_sort(v, 0, v.size() - 1);
print(v);
```

- Vytvára sa vektor `v` so 7 prvkami.
- Najprv sa vektor vypíše neupravene.
- Následne sa triedi pomocou `merge_sort`.
- Potom sa výsledok opäť vypíše.

#### `merge_sort()` – Rekurzívna funkcia

```cpp
void merge_sort(vector<int>& values, int l, int r) {
    if (l < r) {
        int mid = l + (r - l) / 2;
        merge_sort(values, l, mid);
        merge_sort(values, mid + 1, r);
        merge(values, l, mid, r);
    }
}
```

- **Rozdeľuje pole na dve polovice**, kým má viac ako 1 prvok.
- Volá sa rekurzívne pre ľavú a pravú časť.
- Potom zlučuje tieto časti pomocou `merge()`.

#### `merge()` – Zlúčenie dvoch zoradených častí

```cpp
void merge(vector<int>& values, int left_index, int middle_index, int right_index)
```

- **Rozdelí pole na dve časti:**
    - Ľavé podpole: `values[left_index .. middle_index]`
    - Pravé podpole: `values[middle_index+1 .. right_index]`
- Skopíruje tieto dve časti do pomocných polí `left` a `right`.
- Následne ich **zlúči do pôvodného poľa `values` v zoradenom poradí**:
    - Porovnáva prvky z oboch pomocných polí.
    - Vkladá menší do pôvodného poľa.
- Na konci **"dobehne"** zvyšné prvky z jednej alebo druhej časti, ak nejaké zostali.

#### `print()`

```cpp
void print(const vector<int> &v)
```

- Vypíše prvky vektora na obrazovku, oddelené medzerou.
