---
date: '2025-03-22T00:17:32+01:00'
title: 'Prefixový Strom'
---

Prefixový strom (trie) je stromová dátová štruktúra, ktorá ukladá reťazce tak, že každý uzol predstavuje jeden znak
alebo celkový prefix. Vďaka tomu môžu viaceré reťazce zdieľať spoločné prefixy, čo šetrí pamäť a zefektívňuje operácie
ako vyhľadávanie, vkladanie a mazanie. Vyhľadávanie konkrétneho slova prebieha podľa jeho znakov, čo umožňuje operácie v
čase úmernom dĺžke slova, nie celkovému počtu uložených reťazcov.

### Hlavné vlastnosti:

- **Spoločné prefixy:** Viaceré slová s rovnakým začiatkom zdieľajú spoločné uzly.
- **Rýchle vyhľadávanie:** Hľadanie prebieha znak po znaku, čo zabezpečuje rýchlu kontrolu prítomnosti slova.
- **Efektívne ukladanie:** Ukladanie spoločných častí reťazcov znižuje duplicitu.

### Príklad diagramu

Predstavme si, že do prefixového stromu ukladáme slová *"cat"* a *"car"*:

```
        (root)
          |
          c
          |
          a
         / \
        t   r
```

V tomto diagrame:

- Koreňový uzol (root) je prázdny.
- Uzol `c` predstavuje prvý znak všetkých reťazcov.
- Uzol `a` predstavuje spoločný druhý znak.
- Listové uzly `t` a `r` označujú konce slov "cat" a "car".

Tento prístup umožňuje rýchle vyhľadávanie a ukladanie reťazcov, najmä ak je dôležitá hľadaná časová zložitosť nezávislá
od celkového počtu uložených prvkov.

## Implementácia

```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <unordered_map>
#include <utility>

using namespace std;

class TrieNode {
public:
    char label;
    bool isWord = false;
    unordered_map<char, TrieNode *> children;

    TrieNode() = default;

    explicit TrieNode(char label)
        : label(label) {
    }

    TrieNode *get_child(char c) {
        return children[tolower(c)];
    }

    bool has_child(char c) {
        return children[tolower(c)] != nullptr;
    }

    TrieNode *add_child(char c) {
        char l = tolower(c);
        if (!children[l]) children[l] = new TrieNode(l);
        return children[l];
    }

    void remove_child(char c) {
        char l = tolower(c);
        if (!children[l]) return;
        delete children[l];
        children.erase(l);
    }

    ~TrieNode() {
        for (auto [key, node]: children) {
            delete node;
        }
    }
};

class Trie {
    TrieNode *root;

    bool remove_word(const string &value, int idx, TrieNode *node) {
        if (!node) return false;
        if (idx == value.length() - 1) {
            return node->isWord;
        }
        if (node->has_child(value[idx + 1])) {
            bool result = remove_word(value, idx + 1, node->get_child(value[idx + 1]));
            if (result) {
                node->remove_child(value[idx + 1]);
                return !node->isWord && node->children.empty();
            }
            return result;
        }
        return false;
    }

    void get_words(string prefix, vector<string> &words, TrieNode *node) const {
        prefix = prefix + node->label;
        if (node->isWord) words.push_back(prefix);
        for (auto [key, node]: node->children) {
            get_words(prefix, words, node);
        }
    }

public:
    Trie() {
        root = new TrieNode('\0');
    }

    void insert(const string &value) const {
        TrieNode *node = root;
        for (char c: value) {
            node = node->add_child(c);
        }
        node->isWord = true;
    }

    TrieNode *find(const string &value) const {
        TrieNode *node = root;
        for (char c: value) {
            node = node->get_child(c);
            if (!node) return nullptr;
        }
        return node->isWord ? node : nullptr;
    }

    void remove(const string &value) {
        remove_word(value, -1, root);
    }

    void words(vector<string> &words) const {
        for (auto [key, node]: root->children) {
            get_words("", words, node);
        }
    }

    ~Trie() {
        delete root;
    }
};

void print(Trie *trie) {
    vector<string> names;
    trie->words(names);
    for (string name: names) {
        cout << name << " ";
    }
    cout << endl;
}

int main() {
    Trie trie;
    trie.insert("hello");
    trie.insert("world");
    trie.insert("cat");
    trie.insert("car");
    trie.insert("martin");
    trie.insert("martina");
    trie.insert("market");
    trie.insert("margin");

    print(&trie);

    return 0;
}
```

## Vysvetlenie implementácie

Tento kód implementuje prefixový strom (trie), ktorý slúži na ukladanie a vyhľadávanie reťazcov využitím spoločných
prefixov.

### Hlavné časti kódu

**Trieda TrieNode**

- **Atribúty:**
    - `label`: znak, ktorý uzol reprezentuje.
    - `isWord`: boolean príznak, ktorý určuje, či cesta do tohto uzla predstavuje celé slovo.
    - `children`: hašovacia mapa (`unordered_map`), kde kľúčom je znak (prevádzkovaný na malé písmeno) a hodnotou
      ukazovateľ na dieťa.
- **Metódy:**
    - `get_child` a `has_child`: umožňujú prístup k deťom podľa znaku.
    - `add_child`: ak pre daný znak dieťa ešte neexistuje, vytvorí ho a vráti jeho ukazovateľ.
    - `remove_child`: odstráni dieťa z mapy a uvoľní jeho pamäť.
- **Destruktor:**
    - Rekurzívne uvoľní pamäť všetkých potomkov.

**Trieda Trie**

- **Atribút:**
    - `root`: koreňový uzol, inicializovaný s prázdnym znakom (`'\0'`).
- **Metódy:**
    - **insert:** Prechádza zadaný reťazec znak po znaku, vytvára (ak chýbajú) potrebné uzly pomocou `add_child` a na
      konci označí uzol ako slovo (`isWord = true`).
    - **find:** Prechádza uzly podľa znakov reťazca a vráti uzol, ak cesta reprezentuje celé slovo.
    - **remove:** Volá rekurzívnu funkciu `remove_word`, ktorá prejde strom, odstráni dané slovo a zároveň odstráni
      nepotrebné uzly, ak už nepredstavujú žiadne slovo.
    - **words:** Rekurzívne prechádza strom metódou `get_words` a zbiera všetky slová do vektora, pričom postupne
      vytvára reťazec (prefix) reprezentujúci cestu od koreňa k danému uzlu.

**Funkcia print:**

- Získa zoznam všetkých slov uložených v trie a vytlačí ich.

**Funkcia main:**

- Vytvorí inštanciu triedy `Trie` a vloží do nej niekoľko slov (napr. "hello", "world", "cat", "car", "martin", "
  martina", "market", "margin").
- Následne pomocou funkcie `print` zobrazí všetky vložené slová.

### Jednoduchý diagram pre slová "cat" a "car"

```
       (root)
         |
         c
         |
         a
        / \
       t   r
```

V tomto diagrame:

- Cesta `c -> a -> t` reprezentuje slovo "cat" (uzol s `t` má nastavený `isWord = true`).
- Cesta `c -> a -> r` reprezentuje slovo "car".

### Zhrnutie

Kód demonštruje, ako:

- **Vkladať** slová do prefixového stromu pomocou iteratívneho pridávania uzlov, ktoré zdieľajú spoločné prefixy.
- **Vyhľadávať** slová prechádzaním uzlov podľa znakov.
- **Odstraňovať** slová a súčasne čistiť strom od nepotrebných uzlov.
- **Zbierať a vypisovať** všetky slová v strome pomocou rekurzívneho prechádzania.

Týmto spôsobom sa efektívne využíva pamäť, keďže viaceré slová môžu zdieľať spoločné časti, a zároveň sa dosahuje rýchle
vyhľadávanie s časovou zložitosťou závislou len od dĺžky hľadaného slova.
