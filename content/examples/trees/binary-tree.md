---
date: '2025-03-09T18:59:54+01:00'
title: 'Binárny Strom'
---

```cpp
#include <iostream>
#include <utility>

using namespace std;

class Node;

string &traverse_node(Node *node, string *result);

class Node {
private:
    string label;
    Node *parent = nullptr;
    Node *left = nullptr;
    Node *right = nullptr;
public:
    explicit Node(string label) : label(std::move(label)) {}

    int addChild(Node *child) {
        if (left == nullptr) {
            left = child;
            left->setParent(this);
            return -1;
        }
        if (right == nullptr) {
            right = child;
            right->setParent(this);
            return 1;
        }
        cerr << "Cannot add child because parent can hold only two children" << endl;
        return 0;
    }

    const string &getLabel() const {
        return label;
    }

    Node *getParent() const {
        return parent;
    }

    Node *getLeft() const {
        return left;
    }

    Node *getRight() const {
        return right;
    }

    void setParent(Node *parentNode) {
        Node::parent = parentNode;
    }

    string traverse() {
        string output = "";
        output.append(Node::label + ",");
        return traverse_node(this, &output);
    }
};

string &traverse_node(Node *node, string *result) {
    if (!node) return *result;
    if (node->getLeft()) result->append(node->getLeft()->getLabel() + ",");
    if (node->getRight()) result->append(node->getRight()->getLabel() + ",");
    result = &traverse_node(node->getLeft(), result);
    result = &traverse_node(node->getRight(), result);
    return *result;
}

int main() {
    Node root("root"), a("a"), b("b"), c("c");
    a.addChild(&c);
    root.addChild(&a);
    root.addChild(&b);

    string output = root.traverse();
    cout << output << endl;
    return 0;
}
```

Tento kód implementuje jednoduchý binárny strom v C++ s triedou `Node`, ktorá reprezentuje uzly stromu. Obsahuje metódy na pridávanie detí, navigáciu v strome a prechod (traverzovanie) uzlov.

### **Kľúčové časti kódu:**
1. **Trieda `Node`**
    - Má atribúty:
        - `label` – názov uzla.
        - `parent` – smerník na rodičovský uzol.
        - `left`, `right` – smerníky na ľavé a pravé dieťa.
    - Metódy:
        - **`addChild(Node *child)`** – pridá dieťa na ľavú alebo pravú stranu. Ak už má dve deti, vypíše chybu.
        - **`getLabel()`, `getParent()`, `getLeft()`, `getRight()`** – gettery na prístup k údajom.
        - **`traverse()`** – zavolá funkciu `traverse_node()` na prechod stromom.

2. **Funkcia `traverse_node(Node *node, string *result)`**
    - Prechádza binárny strom v **pre-order (NLR)** poradí (navštívi uzol, potom ľavého a nakoniec pravého potomka).
    - Pridáva mená uzlov do reťazca `result`.
    - Rekurzívne volá samú seba na ľavé a pravé podstromy.

3. **Hlavná funkcia `main()`**
    - Vytvorí strom so štyrmi uzlami:
      ```
          root
         /    \
        a      b
       /
      c
      ```
    - Použije `addChild()` na pridanie uzlov `a` a `b` k `root` a `c` k `a`.
    - Zavolá `traverse()` na koreň a vypíše prechádzanie stromom.

---

### **Problém v kóde**
- `traverse()` vracia **referenciu na lokálny reťazec**, čo vedie k **undefined behavior** (použitie uvoľnenej pamäte).
  ```cpp
  string &traverse() {
      string output = "";
      output.append(Node::label + ",");
      return traverse_node(this, &output);
  }
  ```
  **Oprava:** Namiesto návratu referencie by mala funkcia vracať objekt `string`:
  ```cpp
  string traverse() {
      string output = "";
      output.append(Node::label + ",");
      return traverse_node(this, &output);
  }
  ```

### **Výstup programu:**
```
root,a,c,b,
```
čo zodpovedá **pre-order prechodu** stromu.
