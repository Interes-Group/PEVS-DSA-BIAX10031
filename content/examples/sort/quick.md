---
date: '2025-03-24T11:22:07+01:00'
title: 'Quicksort'
---

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
* This function takes the first element as pivot, and places
   all the elements smaller than the pivot on the left side
   and all the elements greater than the pivot on
   the right side. It returns the index of the last element
   on the smaller side.
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
* This function takes the last element as pivot, places
   the pivot element at its correct position in sorted
   array, and places all smaller (smaller than pivot)
   to the left of the pivot and all greater elements
   to the right  of the pivot
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

int partition(vector<int> &values, int low, int high) {
    return lomuto_partition(values, low, high);
}

void quicksort(vector<int> &values, int low, int high) {
    if (low < high) {
        // najdi pivot taky, kde vsetky mensie prvky su vlavo a vacsie prvky su vpravo
        int pivot = partition(values, low, high);

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
    quicksort(v, 0, v.size() - 1);
    print(v);
    return 0;
}
```
