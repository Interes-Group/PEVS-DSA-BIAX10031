---
date: '2025-04-26T18:40:59+02:00'
title: 'Prehľadávanie grafu do hĺbky'
weight: 2
---

## Implementácia

```cpp
#include <iostream>
#include <limits.h>
#include <vector>

using namespace std;

class Graph {
private:
    int **matrix;
    int number_of_vertices;

    void depth_first_traversal(int startVertex, vector<bool> &visited, vector<int> &result) {
        visited[startVertex] = true;
        result.push_back(startVertex);
        for (int i = 0; i < number_of_vertices; i++) {
            if (!visited[i] && matrix[startVertex][i]) {
                depth_first_traversal(i, visited, result);
            }
        }
    }

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

    vector<int> traversal() {
        vector<bool> visited(number_of_vertices, false);
        vector<int> result;
        depth_first_traversal(0, visited, result);
        return result;
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

template<typename Tp>
void print_vector(vector<Tp> &v) {
    for (auto i: v) {
        cout << i << " ";
    }
    cout << endl;
}

int main() {
    Graph graph(4);

    graph.add_edge(0, 1, 1);
    graph.add_edge(0, 2, 5);
    graph.add_edge(1, 2, 2);
    graph.add_edge(2, 0, 5);
    graph.add_edge(2, 3, 3);

    graph.print();
    vector<int> vertices = graph.traversal();
    print_vector(vertices);

    return 0;
}
```

## Vysvetlenie

Kód obsahuje implementáciu prehľadávania DFS (depth-first search) v triede `Graph`.

### 1. Záhlavie a používané knižnice

```cpp
#include <iostream>
#include <limits.h>   // tu nie je vlastne využité
#include <vector>
using namespace std;
```

- `iostream` pre vstup–výstup (`cout`).
- `vector` pre dynamické polia C++ STL.
- `limits.h` sa bežne používa pre konštanty ako `INT_MAX`, ale v tomto príklade sa nevyužíva.

### 2. Členové triedy `Graph`

```cpp
class Graph {
private:
    int **matrix;             // matica susednosti (váhy hrán)
    int number_of_vertices;   // počet vrcholov
    void depth_first_traversal(int startVertex,
                               vector<bool> &visited,
                               vector<int> &result);
public:
    explicit Graph(int numberOfVertices);
    void add_edge(int v1, int v2) const;
    void add_edge(int v1, int v2, int weight) const;
    void remove_edge(int v1, int v2) const;
    vector<int> traversal();
    void print() const;
    ~Graph();
};
```

- **`matrix`**: ukazovateľ na dynamicky alokovanú 2D maticu veľkosti _n×n_.
- **`number_of_vertices`**: počet vrcholov grafu.
- **`depth_first_traversal`**: súkromná rekurzívna funkcia vykonávajúca DFS od zadaného vrcholu.

### 3. Rekurzívna metóda DFS

```cpp
void Graph::depth_first_traversal(int startVertex,
                                  vector<bool> &visited,
                                  vector<int> &result) {
    visited[startVertex] = true;       // označíme vrchol ako navštívený
    result.push_back(startVertex);     // pridáme ho do výsledku

    // prejdeme všetky možné susedné vrcholy
    for (int i = 0; i < number_of_vertices; i++) {
        // ak nie je navštívený a existuje hrana (matica != 0), voláme rekurziu
        if (!visited[i] && matrix[startVertex][i]) {
            depth_first_traversal(i, visited, result);
        }
    }
}
```

- `visited` je pole `bool`, ktoré stráži, ktoré vrcholy už DFS navštívila.
- `result` ukladá poradie navštívenia vrcholov.

### 4. Konštruktor a deštruktor

```cpp
Graph::Graph(int numberOfVertices) {
    number_of_vertices = numberOfVertices;
    matrix = new int*[numberOfVertices];
    for (int i = 0; i < numberOfVertices; i++) {
        matrix[i] = new int[numberOfVertices];
        for (int j = 0; j < numberOfVertices; j++)
            matrix[i][j] = 0;    // inicializácia matice na 0
    }
}

Graph::~Graph() {
    for (int i = 0; i < number_of_vertices; i++)
        delete[] matrix[i];
    delete[] matrix;
}
```

- Konštruktor alokuje maticu susednosti a **nuluje** všetky váhy.
- Deštruktor uvoľní všetku alokovanú pamäť, aby nedošlo k úniku pamäti.

### 5. Pridávanie/odstraňovanie hrán

```cpp
void Graph::add_edge(int v1, int v2) const {
    add_edge(v1, v2, 1);  // štandardná váha 1
}

void Graph::add_edge(int v1, int v2, int weight) const {
    matrix[v1][v2] = weight;
    matrix[v2][v1] = weight;   // neorientovaný graf
}

void Graph::remove_edge(int v1, int v2) const {
    matrix[v1][v2] = 0;
    matrix[v2][v1] = 0;
}
```

- Graf je **neorientovaný**, preto pri pridávaní alebo mazaní hrany meníme obe polia `[v1][v2]` aj `[v2][v1]`.

### 6. Verejná metóda `traversal()`

```cpp
vector<int> Graph::traversal() {
    vector<bool> visited(number_of_vertices, false);
    vector<int> result;
    depth_first_traversal(0, visited, result);  // začneme od vrcholu 0
    return result;
}
```

- Vytvorí pole `visited`, volá DFS od vrcholu `0` a vráti poradie navštívených vrcholov.

### 7. Pomocná funkcia na výpis vektora

```cpp
template<typename Tp>
void print_vector(vector<Tp> &v) {
    for (auto i: v)
        cout << i << " ";
    cout << endl;
}
```

- Jednoduchý šablónový výpis obsahu `vector<T>`.

> [!NOTE]
> V C++ sú šablóny (templates) mechanizmus generického programovania, ktorý umožňuje písať funkcie alebo triedy
> parametrizované typom (alebo aj inými hodnotami). Kompilátor pri použití šablóny automaticky „vygeneruje“ konkrétnu
> verziu pre každý skutočný typ, ktorý sa pri vyvolaní použije.
> [Viac o c++ templates sa môžte dozvedieť tu.](/other/cpp-templates)

### 8. `main()`

```cpp
int main() {
    Graph graph(4);

    graph.add_edge(0, 1, 1);
    graph.add_edge(0, 2, 5);
    graph.add_edge(1, 2, 2);
    graph.add_edge(2, 0, 5);
    graph.add_edge(2, 3, 3);

    graph.print();                         // vypíše maticu susednosti
    vector<int> vertices = graph.traversal();
    print_vector(vertices);                // vypíše: 0 1 2 3

    return 0;
}
```

1. Vytvorí sa graf so 4 vrcholmi.
2. Pridajú sa hrany s vymaníkovanými váhami.
3. `print()` zobrazí:

   ```
   0 : 0 1 5 0 
   1 : 1 0 2 0 
   2 : 5 2 0 3 
   3 : 0 0 3 0 
   ```

4. `traversal()` vykoná DFS od vrcholu 0 a vráti poradie `[0, 1, 2, 3]`.
5. `print_vector` toto poradie vypíše na obrazovku.

## Zhrnutie

- Kód rozširuje základný graf (matica susednosti) o **rekurzívne** DFS.
- DFS označí vrchol `visited`, pridá ho do `result` a pre každý nenavštívený sused pokračuje rekurzívne.
- `traversal()` štartuje DFS od vrcholu 0 a vracia výsledné poradie.
- Pamäťová správa (konštruktor/deštruktor) zabezpečuje, že sa po skončení programu matica uvoľní.
