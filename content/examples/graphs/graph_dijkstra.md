---
date: '2025-04-26T18:41:09+02:00'
title: 'Dijkstra algoritmus'
weight: 3
---

Dijkstrov algoritmus je klasická metóda na nájdenie najkratších ciest z jedného východzieho vrcholu do všetkých
ostatných vrcholov v grafe s nevylučujúcimi sa (netreba orientované ani neorientované) hranami, ktoré majú nezáporné
váhy. Na začiatku sa všetky vzdialenosti od štartovacieho vrcholu považujú za nekonečné okrem samotného počiatočného
vrcholu, ktorého vzdialenosť je nulová. Všetky vrcholy sa ukladajú do množiny nespracovaných, z ktorej počas behu
algoritmu vyberáme ten, ktorý má momentálne najmenšiu odhadovanú vzdialenosť.

Potom opakovane vyberáme nespracovaný vrchol s najmenšou časťou vzdialenosťou — typicky pomocou min-haldy (prioritnej
fronty). Z tohto vrcholu potom „uvolňujeme“ všetky jeho susedné hrany: pre každého suseda skontrolujeme, či cesta cez
práve spracovávaný vrchol vedie k menšej vzdialenosti, než sme doteraz poznali. Ak áno, aktualizujeme odhad vzdialenosti
tohto suseda na novú, menšiu hodnotu a prípadne ho vložíme späť do haldy, aby bol v ďalších krokoch znovu zvolený, ak to
bude potrebné.

Takto postupne spracujeme každý vrchol presne raz, keďže po vybraní z haldy má už jeho konečná najkratšia vzdialenosť
platnú hodnotu a žiadna ďalšia relaxácia už neobsiahne ešte kratšiu cestu. Algoritmus končí v momente, keď je množina
nespracovaných vrcholov prázdna (alebo keď sme docielili konkrétny cieľ, ak hľadáme iba k jednej cieľovej destinácii).
Na výstupe získame pole najkratších vzdialeností od východzieho vrcholu ku každému vrcholu v grafe.

Dijkstrov algoritmus má časovú zložitosť O((V + E) log V) pri použití haldy, kde V je počet vrcholov a E počet hrán
grafu. Vďaka tomu je efektívny aj na väčších grafoch s tisíckami vrcholov, pokiaľ majú hrany nezáporné váhy — čo je
nevyhnutná podmienka pre správnosť algoritmu.

#### **Kľúčové výhody Dijkstrovho algoritmu**

- **Efektívnosť pre neorientované i orientované grafy s nezápornými váhami**  
  Vďaka použitiu prioritnej fronty (min-haldy) dokáže nájsť najkratšie cesty v čase približne O((V+E) log V), čo je
  prakticky použiteľné aj na stredne veľké až väčšie grafy.
- **Jednoduchosť implementácie**  
  Logika „relaxácie“ hrán a výberu vrchola s najmenšou vzdialenosťou je priamočiara a dobre dokumentovaná v literatúre.
- **Kompletnosť výsledkov**  
  Po dokončení beží algoritmus až dovtedy, kým nevyčerpá všetky vrcholy, takže získame garanciu, že pre každý vrchol je
  spočítaná presná najkratšia vzdialenosť od východzého bodu.

#### **Hlavné obmedzenia a nedostatky**

- **Nevhodný pre negatívne váhy**  
  Ak sa v grafe vyskytne záporná hrana, relaxačné kroky môžu viesť k nekonečnému cyklu zlepšovania („záporné cykly“), a
  algoritmus prestane byť korektný.
- **Menej efektívny na veľmi hustých grafoch**  
  Pri veľkom počte hrán (E blízke V²) je reálna zložitosť blízka O(V² log V) alebo vyššia, a priestorová náročnosť (
  najmä ak graf reprezentujeme maticou) sa stáva významnou.
- **Potrebná „globálna“ znalosť celého grafu**  
  Keďže algoritmus paralelne sleduje všetky možné cesty, nie je vhodný, ak máme veľmi veľké alebo dynamicky sa meniace
  grafy, kde by sa skôr hodili lokalizovanejšie či “online” prístupy (napr. A* pre cesty k jednému cieľu, či
  Bellman-Ford pre záporné hrany).

Viac o algoritme sa môžte dozvedieť na odkazoch:

- [https://www.geeksforgeeks.org/introduction-to-dijkstras-shortest-path-algorithm/](https://www.geeksforgeeks.org/introduction-to-dijkstras-shortest-path-algorithm/)
- [https://www.geeksforgeeks.org/printing-paths-dijkstras-shortest-path-algorithm/](https://www.geeksforgeeks.org/printing-paths-dijkstras-shortest-path-algorithm/)

## Implementácia

```cpp
#include <iostream>
#include <limits.h>
#include <queue>
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

    vector<vector<pair<int, int> > > get_adjacencent() const {
        vector<vector<pair<int, int> > > adjacency(number_of_vertices);
        for (int i = 0; i < number_of_vertices; i++) {
            for (int j = 0; j < number_of_vertices; j++) {
                if (matrix[i][j]) {
                    adjacency[i].push_back(make_pair(j, matrix[i][j]));
                    adjacency[j].push_back(make_pair(i, matrix[i][j]));
                }
            }
        }
        return adjacency;
    }

    vector<int> dijkstra(int start) const {
        vector<vector<pair<int, int> > > adjacency = get_adjacencent();
        priority_queue<pair<int, int>, vector<pair<int, int> >, greater<> > pqueue;

        vector<int> distances(number_of_vertices, INT_MAX);
        pqueue.push(make_pair(0, start));
        distances[start] = 0;

        while (!pqueue.empty()) {
            int u = pqueue.top().second;
            pqueue.pop();
            for (auto x: adjacency[u]) {
                int v = x.first;
                int weight = x.second;
                if (distances[v] > distances[u] + weight) {
                    distances[v] = distances[u] + weight;
                    pqueue.push(make_pair(distances[v], v));
                }
            }
        }
        return distances;
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

    vector<int> dist = graph.dijkstra(0);
    print_vector(dist);
    return 0;
}
```

## Vysvetlenie

Nižšie je detailné vysvetlenie celej rozšírenej triedy `Graph` vrátane algoritmov DFS a Dijkstra:

### 1. Záhlavie a knižnice

```cpp
#include <iostream>   // cout, endl
#include <limits.h>   // INT_MAX
#include <queue>      // priority_queue
#include <vector>     // vector
using namespace std;
```

- **`<limits.h>`** definuje `INT_MAX` – použijeme ho ako „nekonečno” pri inicializácii vzdialeností.
- **`<queue>`** prináša `priority_queue`, ktorú použijeme v Dijkstrovom algoritme.
- **`<vector>`** poskytuje dynamické polia STL.

### 2. Členovia triedy `Graph`

```cpp
class Graph {
private:
    int **matrix;               // matica susednosti (n×n)
    int number_of_vertices;     // počet vrcholov

    // súkromná rekurzívna metóda pre DFS
    void depth_first_traversal(int startVertex,
                               vector<bool> &visited,
                               vector<int> &result);

public:
    explicit Graph(int numberOfVertices);
    void add_edge(int v1, int v2) const;
    void add_edge(int v1, int v2, int weight) const;
    void remove_edge(int v1, int v2) const;

    // vykoná DFS od vrcholu 0 a vráti poradie návštev
    vector<int> traversal();

    // zostaví adjacency list z matice
    vector<vector<pair<int,int>>> get_adjacencent() const;

    // Dijkstrov algoritmus na najkratšie cesty od vrcholu start
    vector<int> dijkstra(int start) const;

    void print() const;   // výpis matice susednosti
    ~Graph();             // deštruktor na uvoľnenie pamäte
};
```

### 3. Konštruktor a deštruktor

```cpp
Graph::Graph(int numberOfVertices) {
    number_of_vertices = numberOfVertices;
    matrix = new int*[numberOfVertices];
    for (int i = 0; i < numberOfVertices; i++) {
        matrix[i] = new int[numberOfVertices];
        for (int j = 0; j < numberOfVertices; j++)
            matrix[i][j] = 0;  // žiadna hrana pôvodne
    }
}

Graph::~Graph() {
    for (int i = 0; i < number_of_vertices; i++)
        delete[] matrix[i];
    delete[] matrix;
}
```

- Dynamická alokácia `n` riadkov po `n` stĺpcoch.
- Inicializácia každej bunky na `0` (t. j. žiadna hrana).
- Deštruktor uvoľní všetky riadky a následne samotné pole ukazovateľov.

### 4. Pridávanie a mazanie hrán

```cpp
void Graph::add_edge(int v1, int v2) const {
    add_edge(v1, v2, 1);
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

- **`add_edge(v1, v2)`** volá verziu s váhou 1.
- Vzhľadom na neorientovanosť nastavujeme obe smerové polia.

### 5. Rekurzívny DFS (DFS)

```cpp
void Graph::depth_first_traversal(int u,
                                  vector<bool> &visited,
                                  vector<int> &result) {
    visited[u] = true;
    result.push_back(u);
    for (int v = 0; v < number_of_vertices; v++) {
        if (!visited[v] && matrix[u][v]) {
            depth_first_traversal(v, visited, result);
        }
    }
}
```

- **`visited`**: príznakový vektor, či sme vrchol už videli.
- **`result`**: poradie navštívenia.
- Z každej bunky `u` rekurzívne navštívame všetkých nespracovaných susedov.

#### Verejná obálka

```cpp
vector<int> Graph::traversal() {
    vector<bool> visited(number_of_vertices, false);
    vector<int> result;
    depth_first_traversal(0, visited, result);  // začíname od 0
    return result;
}
```

### 6. Prevod na adjacency list

```cpp
vector<vector<pair<int,int>>> Graph::get_adjacencent() const {
    vector<vector<pair<int,int>>> adjacency(number_of_vertices);
    for (int i = 0; i < number_of_vertices; i++) {
        for (int j = 0; j < number_of_vertices; j++) {
            if (matrix[i][j]) {
                adjacency[i].push_back({j, matrix[i][j]});
                adjacency[j].push_back({i, matrix[i][j]});
            }
        }
    }
    return adjacency;
}
```

- Pre každý nenulový prvok `matrix[i][j]` vložíme dvojicu `(sused, váha)` do zoznamu `adjacency[i]`.
- Zároveň vložíme aj opačný smer `(i, váha)` do `adjacency[j]`, lebo graf je neorientovaný.

### 7. Dijkstrov algoritmus

```cpp
vector<int> Graph::dijkstra(int start) const {
    auto adjacency = get_adjacencent();

    // min-heap podľa vzdialenosti
    priority_queue<
        pair<int,int>,
        vector<pair<int,int>>,
        greater<> 
    > pqueue;

    vector<int> distances(number_of_vertices, INT_MAX);
    distances[start] = 0;
    pqueue.push({0, start});

    while (!pqueue.empty()) {
        auto [dist_u, u] = pqueue.top();
        pqueue.pop();
        // relaxácia všetkých hrán z u
        for (auto &edge : adjacency[u]) {
            int v = edge.first;
            int w = edge.second;
            if (distances[v] > dist_u + w) {
                distances[v] = dist_u + w;
                pqueue.push({distances[v], v});
            }
        }
    }
    return distances;
}
```

1. **Inicializácia**
    - `distances[i] = ∞` (`INT_MAX`), okrem `start`, kde je `0`.
    - `pqueue` je min-heap párov `(vzdialenosť, vrchol)` – to zabezpečí, že vždy spracujeme najbližší ešte nespracovaný
      vrchol.
2. **Hlavný cyklus**
    - Zoberieme vrchol `u` s najmenšou momentálnou vzdialenosťou.
    - Pre každý sused `v` hrany `u→v` s váhou `w` skúsime **relaxovať**:
      ```cpp
      if (dist[v] > dist[u] + w) {
          dist[v] = dist[u] + w;
          pqueue.push({dist[v], v});
      }
      ```
3. Na konci `distances` obsahuje najkratšie vzdialenosti od `start` do každého vrcholu.

### 8. Výpis matice aj vektorov

```cpp
void Graph::print() const {
    for (int i = 0; i < number_of_vertices; i++) {
        cout << i << " : ";
        for (int j = 0; j < number_of_vertices; j++)
            cout << matrix[i][j] << " ";
        cout << endl;
    }
}
```

```cpp
template<typename Tp>
void print_vector(vector<Tp> &v) {
    for (auto x : v)
        cout << x << " ";
    cout << endl;
}
```

- `print_vector` je **funkčná šablóna**, ktorá vie vypísať `vector<Tp>` pre ľubovoľný typ `Tp`.

### 9. Funkcia `main`

```cpp
int main() {
    Graph graph(4);

    graph.add_edge(0, 1, 1);
    graph.add_edge(0, 2, 5);
    graph.add_edge(1, 2, 2);
    graph.add_edge(2, 0, 5);
    graph.add_edge(2, 3, 3);

    graph.print();
    // Matica susednosti:
    // 0 : 0 1 5 0
    // 1 : 1 0 2 0
    // 2 : 5 2 0 3
    // 3 : 0 0 3 0

    auto vertices = graph.traversal();
    print_vector(vertices);  
    // Výstup DFS: 0 1 2 3

    auto dist = graph.dijkstra(0);
    print_vector(dist);      
    // Najkratšie vzdialenosti od 0: napr. 0 1 3 6

    return 0;
}
```

## Zhrnutie

- **DFS** (`depth_first_traversal`) prehľadáva graf do hĺbky a vracia poradie navštívenia.
- **Adjacency list** je odvodený z matice, aby Dijkstra mohol byť efektívnejší.
- **Dijkstra** využíva min-heap (`priority_queue` s `greater<>`) na postupné relaxovanie najbližších vrcholov.
- Šablóna **`print_vector`** uľahčuje výpis akéhokoľvek vektora.
- Celá implementácia ukazuje dynamickú alokáciu, rekurziu, STL kontajnery a klasické grafové algoritmy v čistom C++.
