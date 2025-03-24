---
date: '2025-03-24T11:21:57+01:00'
title: 'Bubble Sort'
---

```cpp
#include <iostream>
#include <vector>

using namespace std;

bool compare(int a, int b, int direction) {
    return direction > 0 ? a > b : a < b;
}

void bubble_sort(vector<int>& values, int direction) {
    size_t size = values.size();

    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (compare(values[j], values[j + 1], direction)) {
                int tmp = values[j];
                values[j] = values[j + 1];
                values[j + 1] = tmp;
            }
        }
    }
}

void print(vector<int>& values) {
    for (int value: values) {
        cout << value << " ";
    }
    cout << endl;
}

int main() {
    vector<int> values = {3, 8, 5, 66, -7};
    print(&values);
    bubble_sort(values, 1);
    print(&values);
    return 0;
}
```
