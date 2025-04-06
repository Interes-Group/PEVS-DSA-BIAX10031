---
date: '2025-04-06T17:48:16+02:00'
title: 'Binárny Vyhľadávací Strom'
---

Binárny vyhľadávací strom (BST) je dátová štruktúra, v ktorej každý uzol má maximálne dvoch potomkov – ľavého a pravého.
Kľúčová vlastnosť BST je, že pre každý uzol platí, že všetky hodnoty v ľavom podstrome sú menšie a všetky hodnoty v
pravom podstrome sú väčšie ako hodnota daného uzla. Táto usporiadaná štruktúra umožňuje veľmi efektívne vyhľadávanie,
vkladanie a mazanie prvkov, pretože rozhodnutie o ďalšom postupe sa robí na základe porovnania hľadanej hodnoty s
hodnotou aktuálneho uzla.

Nižšie je znázornený príklad binárneho vyhľadávacieho stromu:

```
         8
       /   \
      3    10
     / \     \
    1   6    14
       / \   /
      4   7 13
```

V tomto diagrame:

- Uzol s hodnotou 8 je koreň stromu.
- Hodnoty v ľavom podstrome (1, 3, 4, 6, 7) sú menšie ako 8.
- Hodnoty v pravom podstrome (10, 13, 14) sú väčšie ako 8.

Tento usporiadaný prístup zabezpečuje, že operácie ako vyhľadávanie, vkladanie alebo mazanie môžu byť vykonané v
priemernom čase O(log n), čo je veľmi efektívne najmä pri veľkom počte prvkov.

## Implementácia

Implementácia nižšie predstavuje jednoduchý binárny vyhľadávací strom kde označenie uzla stromu je číslo (int).
Implementácia obsahuje funkciu pre vybalansovanie stromu tak, že vráti nový strom s rovnakými uzlami, ktorý je
balansovaný.

```cpp
#include <iostream>
#include <utility>
#include <string>
#include <vector>

using namespace std;

class Node {
public:
    int label = 0;
    int level = 0;
    Node *left = nullptr;
    Node *right = nullptr;

    explicit Node(int label)
        : label(label) {
    }

    Node(int label, int level)
        : label(label),
          level(level) {
    }

    bool isRoot() const {
        return level == 0;
    }

    void insert(int new_label) {
        if (new_label == label) return;
        if (new_label < label) {
            if (left) left->insert(new_label);
            else left = new Node(new_label, level + 1);
        } else {
            if (right) right->insert(new_label);
            else right = new Node(new_label, level + 1);
        }
    }

    Node *find(int query) {
        if (query == label) return this;
        if (query < label) return left ? left->find(query) : nullptr;
        if (query > label) return right ? right->find(query) : nullptr;
    }

    string to_string() {
        return to_string(new string());
    }

    string to_string(string *output) {
        if (!output) return "";
        output->append(string(level, '\t') + std::to_string(label) + "\n");
        if (right) right->to_string(output);
        if (left) left->to_string(output);
        return *output;
    }

    ~Node() {
        delete left;
        delete right;
    }
};

class BinaryTree {
private:
    Node *root = nullptr;

    void traverse_in_order(Node *node, vector<int> &labels) {
        if (!node) return;
        traverse_in_order(node->left, labels);
        labels.push_back(node->label);
        traverse_in_order(node->right, labels);
    }

    void traverse_pre_order(Node *node, vector<int> &labels) {
        if (!node) return;
        labels.push_back(node->label);
        traverse_pre_order(node->left, labels);
        traverse_pre_order(node->right, labels);
    }

    Node *build_balanced_tree(vector<int> &labels, int start, int end, Node *root) {
        if (start > end) return nullptr;
        int mid = (start + end) / 2;
        Node *node;
        if (!root) {
            node = new Node(labels[mid]);
        } else {
            node = root;
            node->insert(labels[mid]);
        }
        build_balanced_tree(labels, start, mid - 1, node);
        build_balanced_tree(labels, mid + 1, end, node);
        return node;
    }

    Node *deleteNode(Node *node, int label) {
        if (!node) return nullptr;

        if (label < node->label) {
            node->left = deleteNode(node->left, label);
            return node;
        }
        if (label > node->label) {
            node->right = deleteNode(node->right, label);
            return node;
        }
        // label je rovnaký tak sa ide vymazať tento node
        // 0 alebo right child
        if (!node->left) {
            Node *tmp = node->right;
            delete node;
            return tmp;
        }
        // je len left child
        if (!node->right) {
            Node *tmp = node->left;
            delete node;
            return tmp;
        }

        // existuju obe deti
        // nájsť inorder nástupcu -> najmenší prvok v pravom podstrome
        // a aj jeho rodiča
        Node *successorParent = node;
        Node *successor = node->right;
        while (successor->left) {
            successorParent = successor;
            successor = successor->left;
        }

        // skopírovať successorov obsah
        node->label = successor->label;

        // vymazať successora
        if (successorParent->left == successor) successorParent->left = successor->right;
        else successorParent->right = successor->right;

        delete successor;
        return node;
    }

    void print_tree(Node *node, string indent, bool last) {
        if (node != nullptr) {
            cout << indent;
            if (last) {
                cout << "R---";
                indent += "   ";
            } else {
                cout << "L---";
                indent += "|  ";
            }
            cout << node->label << "(" << node->level << ")" << endl;
            print_tree(node->left, indent, false);
            print_tree(node->right, indent, true);
        }
    }

public:
    BinaryTree() = default;

    explicit BinaryTree(int root_label) {
        root = new Node(root_label);
    }

    void in_order(vector<int> &labels) {
        if (!root) return;
        traverse_in_order(root, labels);
    }

    void pre_order(vector<int> &labels) {
        if (!root) return;
        traverse_pre_order(root, labels);
    }

    void insert(int label) {
        if (!root) {
            root = new Node(label);
            return;
        }
        root->insert(label);
    }

    Node *find(int label) {
        if (!root) return nullptr;
        return root->find(label);
    }

    string to_string() {
        return root->to_string();
    }

    void balance() {
        vector<int> labels;
        in_order(labels);
        Node *new_root = build_balanced_tree(labels, 0, labels.size() - 1, nullptr);
        delete root;
        root = new_root;
    }

    void remove(int label) {
        if (!root) return;
        root = deleteNode(root, label);
    }

    void print() {
        print_tree(root, "", true);
    }

    ~BinaryTree() {
        delete root;
    }
};

void print(vector<int> &labels) {
    for (int label: labels) {
        cout << label << " ";
    }
    cout << endl;
}

int main() {
    BinaryTree bst;
    bst.insert(50);
    bst.insert(10);
    bst.insert(220);
    bst.insert(33);
    bst.insert(60);
    bst.insert(51);

    vector<int> labels;
    // cout << bst.to_string() << endl;
    bst.print();
    cout << "In order: ";
    bst.in_order(labels);
    print(labels);
    labels.clear();
    cout << "Preorder: ";
    bst.pre_order(labels);
    print(labels);
    labels.clear();

    cout << "--- Balancing ---" << endl;

    bst.balance();
    // cout << bst.to_string() << endl;
    bst.print();
    cout << "In order: ";
    bst.in_order(labels);
    print(labels);
    labels.clear();
    cout << "Preorder: ";
    bst.pre_order(labels);
    print(labels);
    labels.clear();

    cout << "--- Remove 60 ---" << endl;

    bst.remove(60);
    // cout << bst.to_string() << endl;
    bst.print();
    cout << "In order: ";
    bst.in_order(labels);
    print(labels);
    labels.clear();
    cout << "Preorder: ";
    bst.pre_order(labels);
    print(labels);
    labels.clear();

    return 0;
}
```

## Vysvetlenie implementácie

Tento kód implementuje binárny vyhľadávací strom (BST) pomocou tried **Node** a **BinaryTree**. Hlavné myšlienky a
funkcionality sú nasledovné:

### Trieda Node

- **Údaje:** Každý uzol obsahuje:
    - **label:** celá hodnota uzla.
    - **level:** úroveň uzla (hĺbka v strome, kde koreň má level 0).
    - **left** a **right:** ukazovatele na ľavý a pravý podstrom.
- **Vkladanie:** Metóda `insert` vloží nový prvok tak, že:
    - Ak je nová hodnota rovnaká ako aktuálna, nič sa nevykoná (duplicitné hodnoty nie sú vkladané).
    - Ak je nová hodnota menšia, vkladá sa do ľavého podstromu, inak do pravého.
- **Vyhľadávanie:** Metóda `find` rekurzívne hľadá uzol s danou hodnotou.
- **Zobrazenie:** Metódy `to_string` (s rôznymi variantami) a `print_tree` slúžia na generovanie textového zobrazenia
  stromu.
- **Destrukcia:** V deštruktore sa rekurzívne uvoľňujú pamäťové zdroje všetkých potomkov.

### Trieda BinaryTree

- **Správa stromu:** Obsahuje ukazovateľ na koreň stromu a poskytuje metódy na manipuláciu s celým stromom.
- **Prechádzania:**
    - **In-order:** Rekurzívne prechádza strom (ľavý podstrom, uzol, pravý podstrom), čím zaručuje zoradené hodnoty.
    - **Pre-order:** Navštívi uzol pred jeho podstromami.
- **Vkladanie a vyhľadávanie:** Metódy `insert` a `find` volajú príslušné metódy triedy Node.
- **Balansovanie stromu:**
    - Metóda `balance` najprv vykoná in-order prechod, čím získa zoradený zoznam hodnôt.
    - Následne pomocou funkcie `build_balanced_tree` vytvorí vyvážený strom, ktorý minimalizuje hĺbku a optimalizuje
      operácie vyhľadávania.
- **Mazanie uzlov:**
    - Funkcia `deleteNode` rekurzívne hľadá uzol s danou hodnotou.
    - Rieši tri prípady:
        - Uzol nemá ľavého podstroma (nahradí sa pravým).
        - Uzol nemá pravého podstroma (nahradí sa ľavým).
        - Uzol má oba podstromy – v tomto prípade sa nájde in-order nástupca (najmenší uzol v pravom podstrome), jeho
          hodnota sa skopíruje a následne sa tento nástupca vymaže.
- **Zobrazenie stromu:** Metóda `print` volá rekurzívnu funkciu `print_tree`, ktorá zobrazuje strom s odsadením podľa
  hĺbky.

### Priebeh programu (main)

1. **Vytvorenie a plnenie stromu:**
    - Vytvorí sa prázdny strom.
    - Postupne sa do stromu vložia hodnoty: 50, 10, 220, 33, 60, 51.
2. **Výpis pôvodného stromu:**
    - Pomocou `print` sa zobrazí grafická reprezentácia stromu.
    - Vykonajú sa in-order a pre-order prechody a ich výsledky sa vytlačia.
3. **Balansovanie stromu:**
    - Metóda `balance` preusporiada strom na základe zoradeného zoznamu hodnôt.
    - Vyvážený strom je opäť zobrazený spolu s jeho prechodmi.
4. **Mazanie uzla:**
    - Uzol s hodnotou 60 sa odstráni pomocou metódy `remove`.
    - Po odstránení sa opäť zobrazí strom a jeho prechody.

### Diagramy

**Pôvodný strom po vložení hodnôt:**

```
         50 (lvl 0)
        /         \
 10 (lvl 1)       220 (lvl 1)
       \             /
    33 (lvl 2)  60 (lvl 2)
                  /
             51 (lvl 3)
```

**Strom po vyvážení:**

```
        50 (lvl 0)
       /         \
10 (lvl 1)      60 (lvl 1)
    \           /         \
33 (lvl 2)   51 (lvl 2)  220 (lvl 2)
```

**Strom po odstránení uzla s hodnotou 60:**

*(Pri mazaniu uzla s dvoma deťmi sa nahradí hodnota 60 hodnotou in-order nástupcu, v tomto prípade 220)*

```
        50 (lvl 0)
       /         \
10 (lvl 1)      220 (lvl 1)
    \           /
33 (lvl 2)    51 (lvl 2)
```

### Zhrnutie

Kód demonštruje, ako:

- Vkladať a vyhľadávať prvky v binárnom vyhľadávacom strome,
- Vykonávať rôzne prechádzacie metódy (in-order a pre-order),
- Balansovať strom, aby sa zlepšila efektivita operácií,
- Odstraňovať uzly a správne upravovať strom tak, aby zachoval vlastnosti BST.

Týmto spôsobom sa zabezpečuje, že operácie vyhľadávania, vkladania a mazania môžu prebiehať efektívne, pričom vyvážený
strom minimalizuje hĺbku a tým aj čas potrebný na vykonanie týchto operácií.
