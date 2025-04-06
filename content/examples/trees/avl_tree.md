---
date: '2025-04-06T17:49:03+02:00'
title: 'AVL Strom'
---

AVL strom je typ binárneho vyhľadávacieho stromu, ktorý sa automaticky udržiava vyvážený. Hlavnou vlastnosťou AVL stromu
je, že pre každý uzol je rozdiel výšok jeho ľavého a pravého podstromu (tzv. balance factor) maximálne 1. Tento balans
sa zabezpečuje automatickými operáciami rotácií po vkladaní alebo odstraňovaní uzlov, čím sa udržuje optimálna hĺbka
stromu a zaručuje rýchle vyhľadávanie.

### Kľúčové body AVL stromu

- **Vyváženosť:** Rozdiel medzi výškou ľavého a pravého podstromu každého uzla je -1, 0 alebo 1.
- **Rotácie:** Ak v dôsledku vkladania alebo mazania dôjde k porušeniu vyváženosti, strom sa rebalansuje pomocou jednej
  z nasledujúcich rotácií:
    - Jednoduchá pravá rotácia (RR rotácia)
    - Jednoduchá ľavá rotácia (LL rotácia)
    - Dvojitá ľavá-pravá rotácia (LR rotácia)
    - Dvojitá pravá-ľavá rotácia (RL rotácia)
- **Efektivita:** Vďaka vyváženosti má AVL strom časovú zložitosť O(log n) pre operácie ako vyhľadávanie, vkladanie a
  mazanie.

### Diagramy

**1. Príklad vyváženého AVL stromu:**

```
         30
       /    \
     20      40
    /  \    /  \
  10   25  35   50
```

V tomto strome má každý uzol vyvážené podstromy, čo umožňuje rýchle vyhľadávanie.

**2. Príklad rebalansovania – jednoduchá pravá rotácia:**

*Nevyvážený strom:*

```
      30
     /
   20
  /
10
```

*Po pravom otočení (rotácii):*

```
    20
   /  \
 10    30
```

Týmto spôsobom sa zníži hĺbka stromu a vyváženosť sa obnoví.

**3. Príklad rebalansovania – jednoduchá ľavá rotácia:**

*Nevyvážený strom:*

```
   10
     \
      20
        \
         30
```

*Po ľavom otočení (rotácii):*

```
      20
     /  \
   10    30
```

Tieto rotácie zabezpečujú, že AVL strom zostáva vyvážený aj po dynamických zmenách, čím optimalizuje všetky operácie
spojené s vyhľadávaním a úpravou dát.

## Implementácia

```cpp
#include <iostream>
#include <utility>
#include <string>
#include <vector>

using namespace std;

class Node {
public:
    int label = 0;
    int height = 1;
    int balance = 0; // má mať len hodnoty 1 0 -1

    Node *left = nullptr;
    Node *right = nullptr;

    explicit Node(int label)
        : label(label) {
    }

    static int getHeight(Node *node) {
        return node ? node->height : 0;
    }

    static int getBalance(Node *node) {
        if (!node) return 0;
        int balance = getHeight(node->left) - getHeight(node->right);
        node->balance = balance;
        return balance;
    }

    Node *rotateRight() {
        Node *left_subtree = left;
        Node *right_left_subtree = left_subtree->right;

        left_subtree->right = this;
        left = right_left_subtree;

        height = max(getHeight(left), getHeight(right)) + 1;
        left_subtree->height = max(getHeight(left_subtree->left), getHeight(left_subtree->right)) + 1;
        return left_subtree;
    }

    Node *rotateLeft() {
        Node *right_subtree = right;
        Node *left_right_subtree = right_subtree->left;

        right_subtree->left = this;
        right = left_right_subtree;

        height = max(getHeight(right), getHeight(left)) + 1;
        right_subtree->height = max(getHeight(right_subtree->right), getHeight(right_subtree->left)) + 1;
        return right_subtree;
    }

    Node *getMinimum() {
        Node *current = this;
        while (current->left != nullptr) {
            current = current->left;
        }
        return current;
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
        output->append(std::to_string(label) + "\n");
        if (left) left->to_string(output);
        if (right) right->to_string(output);
        return *output;
    }

    ~Node() {
        delete left;
        delete right;
    }
};

class AVL_Tree {
    Node *root;

    Node *insertNode(Node *node, int new_label) {
        if (!node) return new Node(new_label);
        if (new_label < node->label) {
            node->left = insertNode(node->left, new_label);
        } else if (new_label > node->label) {
            node->right = insertNode(node->right, new_label);
        } else {
            return node;
        }

        // balancing časť
        node->height = max(Node::getHeight(node->left), Node::getHeight(node->right)) + 1;
        int balance = Node::getBalance(node);

        if (balance > 1) {
            if (new_label < node->left->label) {
                // left left -> stačí jedna right rotation
                return node->rotateRight();
            } else if (new_label > node->left->label) {
                // left right -> najprv left rotation a potom right rotation
                node->left = node->left->rotateLeft();
                return node->rotateRight();
            }
        }
        if (balance < -1) {
            if (new_label > node->right->label) {
                // right right -> stačí jedna left rotation
                return node->rotateLeft();
            } else if (new_label < node->right->label) {
                // right left -> najprv right rotation a potom left rotation
                node->right = node->right->rotateRight();
                return node->rotateLeft();
            }
        }
        return node;
    }

    Node *deleteNode(Node *node, int label) {
        if (!node) return nullptr;
        if (label < node->label) {
            node->left = deleteNode(node->left, label);
        } else if (label > node->label) {
            node->right = deleteNode(node->right, label);
        } else {
            if (!node->left || !node->right) {
                Node *tmp = node->left ? node->left : node->right;
                if (!tmp) {
                    tmp = node;
                    node = nullptr;
                } else {
                    *node = *tmp;
                }
                delete tmp;
            } else {
                Node *tmp = node->right->getMinimum();
                node->label = tmp->label;
                node->right = deleteNode(node->right, tmp->label);
            }
        }

        if (!node) return nullptr;

        node->height = max(Node::getHeight(node->left), Node::getHeight(node->right)) + 1;
        int balance = Node::getBalance(node);

        if (balance > 1) {
            if (Node::getBalance(node->left) >= 0) {
                // left left -> stačí jedna right rotation
                return node->rotateRight();
            } else {
                // left right -> najprv left rotation a potom right rotation
                node->left = node->left->rotateLeft();
                return node->rotateRight();
            }
        }
        if (balance < -1) {
            if (Node::getBalance(node->right) <= 0) {
                // right right -> stačí jedna left rotation
                return node->rotateLeft();
            } else {
                // right left -> najprv right rotation a potom left rotation
                node->right = node->right->rotateRight();
                return node->rotateLeft();
            }
        }
        return node;
    }

    void traverse_in_order(Node *root, vector<int> &labels) {
        if (!root) return;
        traverse_in_order(root->left, labels);
        labels.push_back(root->label);
        traverse_in_order(root->right, labels);
    }

    void traverse_pre_order(Node *root, vector<int> &labels) {
        if (!root) return;
        labels.push_back(root->label);
        traverse_pre_order(root->left, labels);
        traverse_pre_order(root->right, labels);
    }

    void print_tree(Node *node, string indent, bool last) {
        if (!node) return;
        cout << indent;
        if (last) {
            cout << "R----";
            indent += "   ";
        } else {
            cout << "L----";
            indent += "|  ";
        }
        cout << node->label << "(" << node->height << "," << node->balance << ")" << endl;
        print_tree(node->left, indent, false);
        print_tree(node->right, indent, true);
    }

public:
    AVL_Tree() = default;

    explicit AVL_Tree(int root_label) {
        root = new Node(root_label);
    }

    void insert(int new_label) {
        root = insertNode(root, new_label);
    }

    void remove(int label) {
        root = deleteNode(root, label);
    }

    void inorder(vector<int> &labels) {
        traverse_in_order(root, labels);
    }

    void preorder(vector<int> &labels) {
        traverse_pre_order(root, labels);
    }

    void print() {
        print_tree(root, "", true);
    }

    ~AVL_Tree() {
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
    AVL_Tree avl(50);

    avl.insert(10);
    avl.insert(222);
    avl.insert(33);
    avl.insert(60);
    avl.insert(51);

    avl.print();
    cout << "------------------" << endl;

    // pridanie naschvál po sebe idúcich malých prvkov,
    // ktoré by v nebalancovanom binárnom strome spôsobili dlhú reťaz na jeden strane stromu
    avl.insert(5);
    avl.insert(3);
    avl.insert(2);
    avl.insert(1);
    avl.insert(6);
    avl.insert(7);

    avl.print();

    return 0;
}
```

## Vysvetlenie implementácie

Tento kód implementuje AVL strom – samovyvažujúci sa binárny vyhľadávací strom, v ktorom sa udržiava vyváženosť stromu
prostredníctvom rotácií. Hlavné časti kódu a ich funkcie sú nasledovné:

### Trieda Node

- **Atribúty:**
    - **label:** Hodnota (kľúč) uzla.
    - **height:** Výška uzla, ktorá je aktualizovaná po každej zmene stromu.
    - **balance:** Vyváženosť uzla – rozdiel výšok ľavého a pravého podstromu (očakáva sa hodnota -1, 0 alebo 1).
    - **left, right:** Ukazovatele na ľavý a pravý podstrom.

- **Pomocné metódy:**
    - **getHeight(Node\*):** Vracia výšku daného uzla (alebo 0, ak je uzol prázdny).
    - **getBalance(Node\*):** Vypočíta a nastaví balance factor (rozdiel medzi výškou ľavého a pravého podstromu).
    - **rotateRight() a rotateLeft():** Realizujú rotácie, ktoré sú základným mechanizmom na obnovu vyváženosti stromu.
        - Pri **pravom otočení** sa ľavý podstrom stane novým koreňom a pôvodný uzol sa stane pravým potomkom.
        - Pri **ľavom otočení** sa pravý podstrom stane novým koreňom a pôvodný uzol sa stane ľavým potomkom.
    - **getMinimum():** Nájde uzol s najmenšou hodnotou v danom podstrome (používa sa pri mazania uzlov s dvoma deťmi).
    - **find(int):** Hľadá uzol s danou hodnotou v stromovej štruktúre.
    - **to_string():** Vytvorí reťazcové zobrazenie uzlov (jednoduchý prehľad stromu).

- **Destruktor:** Uvoľní dynamicky alokovanú pamäť pre podstromy.

### Trieda AVL_Tree

- **Hlavný prvok:** Obsahuje ukazovateľ na koreň stromu.

- **Metódy na úpravu stromu:**
    - **insertNode(Node\*, int):** Rekurzívna metóda na vloženie nového prvku. Po vložení:
        - Aktualizuje sa výška uzla.
        - Vypočíta sa balance factor.
        - Ak balance prekročí povolený rozsah (väčší ako 1 alebo menší ako –1), vykonajú sa príslušné rotácie:
            - **Left Left (LL):** Stačí jedna pravá rotácia.
            - **Left Right (LR):** Najprv ľavá rotácia na ľavom potomkovi, potom pravá rotácia.
            - **Right Right (RR):** Stačí jedna ľavá rotácia.
            - **Right Left (RL):** Najprv pravá rotácia na pravom potomkovi, potom ľavá rotácia.
    - **deleteNode(Node\*, int):** Rekurzívna metóda na odstránenie uzla:
        - Vyhľadá uzol, ktorý sa má odstrániť.
        - Ak má uzol menej ako dvoch detí, odstráni sa priamo (priradí sa mu dieťa alebo sa nastaví na nullptr).
        - Ak má uzol obidve deti, nájde sa jeho in-order nástupca (najmenší prvok v pravom podstrome), ktorého hodnota
          sa skopíruje, a následne sa tento nástupca odstráni.
        - Po odstránení sa opäť aktualizuje výška a balance factor a v prípade potreby sa vykonajú rotácie.

- **Prechádzacie metódy:**
    - **inorder() a preorder():** Rekurzívne prechádzajú strom a ukladajú hodnoty do vektora.

- **print_tree():** Vypíše strom v prehľadnej hierarchickej štruktúre, pričom každý uzol zobrazuje svoju hodnotu, výšku
  a balance factor.

- **Destruktor:** Uvoľní pamäť celého stromu.

### Priebeh programu (funkcia main)

1. **Vytvorenie stromu:**  
   Vytvorí sa AVL strom s koreňom 50.

2. **Vkladanie prvkov:**  
   Postupne sa vkladajú hodnoty (10, 222, 33, 60, 51), pričom sa pri každom vkladaní kontroluje a obnovuje vyváženosť
   stromu.

3. **Zobrazenie stromu:**  
   Pomocou metódy `print()` sa zobrazí aktuálna štruktúra stromu s informáciami o výške a balance factori.

4. **Ďalšie vkladanie:**  
   Vkladajú sa hodnoty (5, 3, 2, 1, 6, 7), ktoré by v obyčajnom (nevyváženom) strome spôsobili dlhý reťaz, ale AVL strom
   vďaka rotáciám zostáva vyvážený.

### Diagramy

**1. Príklad rotácie – Pravá rotácia (LL prípad):**

*Nevyvážený stav (ľavý podstrom je príliš hlboký):*

```
      X
     /
    Y
   /
  Z
```

*Po pravom otočení (rotácia):*

```
     Y
    / \
   Z   X
```

**2. Príklad rotácie – Ľavá rotácia (RR prípad):**

*Nevyvážený stav (pravý podstrom je príliš hlboký):*

```
  X
   \
    Y
     \
      Z
```

*Po ľavom otočení (rotácia):*

```
    Y
   / \
  X   Z
```

Tieto diagramy ilustrujú základný princíp, ako AVL strom využíva rotácie na obnovenie rovnováhy, aby sa zabezpečilo, že
rozdiel výšok medzi podstromami každého uzla zostáva v medziach ±1.

### Zhrnutie

Kód demonštruje, ako:

- **Vkladať a odstraňovať** prvky v AVL strome,
- **Aktualizovať** výšku a balance factor po každej zmene,
- **Používať rotácie** (pravú a ľavú) na automatické vyváženie stromu,
- **Zobrazovať** štruktúru stromu pre lepšie pochopenie jeho dynamiky.

AVL strom tak zabezpečuje, že operácie vyhľadávania, vkladania a mazania zostávajú efektívne s časovou zložitosťou O(log
n), aj keď sa do stromu vkladajú prvky v nevyváženom poradí.
