---
date: '2025-03-24T11:22:01+01:00'
title: 'Merge Sort'
---

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
