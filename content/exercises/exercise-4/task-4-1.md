---
date: '2025-03-09T17:27:51+01:00'
title: 'Úloha 4.1'
weight: 1
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte **N-árny strom**, t.j. strom ktorého uzol môže mať 0 až N detí. Každý uzol stromu má označenie '**label**'
typu `string`. **Deti** uzla musia byť **unikátne**. Uzol implementujte ako triedu. Aplikujte princípy zaprúzdrenia (private
atribúty, getter, setter a ďalšie).
Pre uzol implementujte metódu:

- `bool add_child(Node*)` - Pridá do uzla nový detský uzol. Metóda **pridá uzol ako dieťa** len ak poskytnutý uzol **ešte nie je
  medzi detskými** uzlami. Metóda vráti true ak bol uzol úspešne pridaný medzi deti, inak vráti false.

Implementujte metódu `string &traverse(Node*)`, ktorá bude implementovať **prechádzanie stromu do hĺbky**. Metódu
implementujte pomocou **rekurzie**. Metóda vráti `string`, ktorý obsahuje označenie uzlov, cez ktoré algoritmus prechádzal
oddelené čiarkou.

Pre demonštráciu vypracovania vytvorte strom aspoň hĺbky 3 s ľubovolným počtom uzlov. Následne zavolajte metódu `traverse()`
a výstup metódy vypíšte na štandardný výstup (`cout`).

### Príklady vstupov / výstupov programu

Majme strom:

![strom](/images/task41-tree.png)

Ak je strom implementovaný podľa zadania, tak výstup metódy traverse je 

```
1, 2, 5, 6, 7, 8, 4, 3
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

using namespace std;

class Node {
private:
    string label;
    vector<Node*> children;

public:
    // Konštruktor
    explicit Node(const string& label) : label(label) {}

    // Getter pre label
    const string& get_label() const {
        return label;
    }

    // Setter pre label
    void set_label(const string& new_label) {
        label = new_label;
    }

    // Getter pre deti
    const vector<Node*>& get_children() const {
        return children;
    }

    // Pridá dieťa, ak ešte nie je medzi deťmi (porovnáva pointer aj label)
    bool add_child(Node* child) {
        if (child == nullptr) return false;

        for (size_t i = 0; i < children.size(); ++i) {
            if (children[i] == child || children[i]->get_label() == child->get_label()) {
                return false; // už existuje
            }
        }

        children.push_back(child);
        return true;
    }
};

// Rekurzívna funkcia na DFS prechod stromom
string traverse(Node* node) {
    if (node == nullptr) return "";

    string result = node->get_label();

    const vector<Node*>& children = node->get_children();
    for (size_t i = 0; i < children.size(); ++i) {
        string sub = traverse(children[i]);
        if (!sub.empty()) {
            result += "," + sub;
        }
    }

    return result;
}

int main() {
    // Vytvorenie uzlov
    Node* root = new Node("A");
    Node* b = new Node("B");
    Node* c = new Node("C");
    Node* d = new Node("D");
    Node* e = new Node("E");
    Node* f = new Node("F");
    Node* g = new Node("G");

    // Budovanie stromu
    root->add_child(b);
    root->add_child(c);
    root->add_child(d);

    b->add_child(e);
    b->add_child(f);

    d->add_child(g);

    // Prechod stromu
    string output = traverse(root);
    cout << "DFS Traversal: " << output << endl;

    // Uvoľnenie pamäte (keďže nepoužívame smart pointery)
    delete root;
    delete b;
    delete c;
    delete d;
    delete e;
    delete f;
    delete g;

    return 0;
}
```

### Vysvetlenie kódu

#### 1. Trieda `Node`
Predstavuje jeden uzol stromu. Obsahuje:
- `label` – názov/označenie uzla (napr. `"A"`, `"B"`, atď.)
- `children` – zoznam (vektor) ukazovateľov na detské uzly

##### Dôležité metódy:
- **`get_label()`** a **`set_label()`** – prístup k názvu uzla
- **`get_children()`** – prístup k zoznamu detí
- **`add_child(Node*)`** – pridanie dieťaťa, len ak ešte neexistuje (podľa pointera alebo mena)

#### 2. Funkcia `traverse(Node*)`
Implementuje **pre-order** (do hĺbky) prechod stromom:
- Najprv navštívi aktuálny uzol
- Potom rekurzívne prechádza všetky deti uzla
- Výsledok je jeden reťazec s názvami uzlov oddelenými čiarkou

#### 3. Funkcia `main()`
- Vytvorí uzly stromu
- Poskladá strom pomocou `add_child()`
- Zavolá `traverse()` na koreň
- Vypíše výsledok na štandardný výstup
- Uvoľní alokovanú pamäť (`delete`)

### Vizualizácia stromu

Strom vyzerá takto (každý uzol je označený písmenom):

```
         A
       / | \
      B  C  D
     / \     \
    E   F     G
```

- **Koreň** je `A`
- `A` má 3 deti: `B`, `C`, `D`
- `B` má 2 deti: `E`, `F`
- `D` má 1 dieťa: `G`
- `C`, `E`, `F`, `G` sú listy (nemajú deti)

### Výstup funkcie `traverse(root)`
Prechod do hĺbky (pre-order) navštívi uzly v tomto poradí:
```
A → B → E → F → C → D → G
```

Čiže:
```
DFS Traversal: A,B,E,F,C,D,G
```

-->

{{< /details >}}
