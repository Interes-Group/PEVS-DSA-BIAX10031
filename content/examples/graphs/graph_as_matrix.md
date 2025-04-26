---
date: '2025-04-26T18:40:40+02:00'
title: 'Graf ako matica'
weight: 1
---

## Implementácia

```cpp
#include <iostream>

using namespace std;

class Graph {
private:
    int **matrix;
    int number_of_vertices;

public:
    explicit Graph(int numberOfVertices) {
        this->number_of_vertices = numberOfVertices;
        matrix = new int *[numberOfVertices];
        for (int i = 0; i < numberOfVertices; i++) {
            matrix[i] = new int[numberOfVertices];
            for (int j = 0; j < numberOfVertices; j++) {
                matrix[i][j] = 0;
            }
        }
    }

    void add_edge(int vertex1, int vertex2) const {
        add_edge(vertex1, vertex2, 1);
    }

    void add_edge(int vertex1, int vertex2, int weight) const {
        matrix[vertex1][vertex2] = weight;
        matrix[vertex2][vertex1] = weight;
    }

    void remove_edge(int vertex1, int vertex2) const {
        matrix[vertex1][vertex2] = 0;
        matrix[vertex2][vertex1] = 0;
    }

    void print() const {
        for (int i = 0; i < number_of_vertices; i++) {
            cout << i << " : ";
            for (int j = 0; j < number_of_vertices; j++) {
                cout << matrix[i][j] << " ";
            }
            cout << endl;
        }
    }

    ~Graph() {
        for (int i = 0; i < number_of_vertices; i++) {
            delete[] matrix[i];
        }
        delete[] matrix;
    }
};

int main() {
    Graph graph(4);

    graph.add_edge(0, 1, 1);
    graph.add_edge(0, 2, 5);
    graph.add_edge(1, 2, 2);
    graph.add_edge(2, 0, 5);
    graph.add_edge(2, 3, 3);

    graph.print();
    
    return 0;
}
```

## Vysvetlenie

### 1. Základné nastavenie

```cpp
#include <iostream>
using namespace std;
```

- `#include <iostream>` – umožňuje použiť vstupno-výstupné toky (`cin`, `cout`).
- `using namespace std;` – zjednodušuje písanie (netreba každýkrát písať `std::cout`, stačí `cout`).

### 2. Deklarácia triedy `Graph`

```cpp
class Graph {
private:
    int **matrix;
    int number_of_vertices;
```

- **`matrix`** – dvojitý ukazovateľ na `int`, bude ukazovať na dynamicky alokovanú 2D maticu (rozmer `n × n`).
- **`number_of_vertices`** – počet vrcholov v grafe.

### 3. Konštruktor

```cpp
explicit Graph(int numberOfVertices) {
    this->number_of_vertices = numberOfVertices;
    matrix = new int *[numberOfVertices];
    for (int i = 0; i < numberOfVertices; i++) {
        matrix[i] = new int[numberOfVertices];
        for (int j = 0; j < numberOfVertices; j++) {
            matrix[i][j] = 0;
        }
    }
}
```

1. Uložíme si počet vrcholov.
2. Dynamicky vytvoríme pole `matrix` dĺžky `numberOfVertices`, ktoré bude držať ukazovatele na riadky.
3. Pre každý riadok `i`:
    - alokujeme pole `int` dĺžky `numberOfVertices`,
    - inicializujeme všetky položky na `0` (t. j. žiadna hrana).

### 4. Pridávanie a odstraňovanie hrán

#### 4.1 Pridanie hrany so štandardnou váhou

```cpp
void add_edge(int vertex1, int vertex2) const {
    add_edge(vertex1, vertex2, 1);
}
```

- Zavolá preťaženú verziu s váhou `1`.

#### 4.2 Pridanie hrany s vlastnou váhou

```cpp
void add_edge(int vertex1, int vertex2, int weight) const {
    matrix[vertex1][vertex2] = weight;
    matrix[vertex2][vertex1] = weight;
}
```

- Graf je **neorientovaný**, preto nastavíme oba smery:  
  `vertex1 → vertex2` aj `vertex2 → vertex1`.

#### 4.3 Odstránenie hrany

```cpp
void remove_edge(int vertex1, int vertex2) const {
    matrix[vertex1][vertex2] = 0;
    matrix[vertex2][vertex1] = 0;
}
```

- Stačí vynulovať obe pozície v matici.

> [!NOTE]
> **Poznámka k `const`**: metódy majú kvalifikátor `const`, ktorý hovorí, že nemenia vlastnosti objektu. V tomto prípade
> však kvalifikátor nevzťahuje priamo na obsah matice (iba na ukazovateľ), takže úprava `matrix[i][j]` prebehne bez chyby
> kompilátora.

### 5. Výpis matice susednosti

```cpp
void print() const {
    for (int i = 0; i < number_of_vertices; i++) {
        cout << i << " : ";
        for (int j = 0; j < number_of_vertices; j++) {
            cout << matrix[i][j] << " ";
        }
        cout << endl;
    }
}
```

- Pre každý vrchol `i` vypíšeme jeho index a potom v riadku hodnoty od `0` do `n−1`, ktoré predstavujú váhy hrán.

**Príklad výstupu pre 4 vrcholy:**

```
0 : 0 1 5 0 
1 : 1 0 2 0 
2 : 5 2 0 3 
3 : 0 0 3 0 
```

### 6. Deštruktor

```cpp
~Graph() {
    for (int i = 0; i < number_of_vertices; i++) {
        delete[] matrix[i];
    }
    delete[] matrix;
}
```

- Uvoľníme pamäť zakúpenú konštruktorom:
    1. Vymažeme každý riadok (`delete[] matrix[i]`).
    2. Vymažeme pole ukazovateľov (`delete[] matrix`).

### 7. Funkcia `main`

```cpp
int main() {
    Graph graph(4);

    graph.add_edge(0, 1, 1);
    graph.add_edge(0, 2, 5);
    graph.add_edge(1, 2, 2);
    graph.add_edge(2, 0, 5);
    graph.add_edge(2, 3, 3);

    graph.print();
    
    return 0;
}
```

1. Vytvoríme graf so 4 vrcholmi.
2. Pridáme hrany (napr. `0–1` s váhou 1, `0–2` s váhou 5, atď.).
3. Zavoláme `print()`, ktorý zobrazí celú maticu.
4. Pri ukončení funkcie `main` sa automaticky zavolá deštruktor `~Graph()`, ktorý uvoľní alokovanú pamäť.

## Zhrnutie

- **Reprezentácia grafu**: matica susednosti `n × n`, kde `matrix[i][j]` = váha hrany medzi vrcholmi `i` a `j` (alebo
  `0`, ak hrana neexistuje).
- **Výhody**: rýchly prístup k informácii, či existuje hrana medzi dvoma vrcholmi (O(1)).
- **Nevýhody**: pamäťová náročnosť O(n²), aj keď málo hrán.

Tento jednoduchý príklad perfektne demonštruje dynamickú alokáciu, zodpovednosť za uvoľnenie pamäte a základné operácie
nad neorientovaným váženým grafom.
