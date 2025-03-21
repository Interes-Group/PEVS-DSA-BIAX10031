---
date: '2025-03-09T17:27:54+01:00'
title: 'Úloha 4.2'
weight: 2
---

**Trie (prefixový strom)** je efektívna dátová štruktúra na ukladanie a vyhľadávanie reťazcov, najmä keď pracujeme s
veľkým množstvom slov so spoločnými prefixami. Každý uzol v strome reprezentuje jedno písmeno a cesty od koreňa k listom
tvoria uložené slová. Trie umožňuje rýchle vyhľadávanie, vkladanie aj mazanie slov v čase O(m), kde *m* je dĺžka slova,
a využíva sa napríklad v autokompletizácii, slovníkoch a indexovaní textu. Čím viac slov je uložených v štruktúre tím je
efektívnejšia.

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Implementujte **prefixový strom, Trie**. Strom implementujte pomocou tried a dbajte na dodržanie princípov
zaprúzdrenia (private atribúty, getter, setter a ďalšie). Každý uzol stromu, až na koreňový uzol (root),
má označenie '**label**' jedno písmeno (typ `char`). Prefixový strom je N-árny strom. Každý uzol obsahuje
aj označenie typu `bool` či ide **o ukončenie slova**. Ak má uzol označenia konca slova hodnotu `true`,
tak existuje cesta v strome (t.j. slovo), ktorého písmeno uzla je posledné písmeno a tak cesta k danému uzlu
predstavuje indexované slovo v strome.

> [!IMPORTANT]
> Dobre si rozmyslite ako implementujete kontajner pre uchovanie detí uzla, dbajte na efektívne prehľadávanie.

Implementujte metódy stromu:

- `bool insert(string word)` - Vloží nové slovo do stromu. Metóda vráti hodnotu `true` ak bolo slovo úspešne vložené do
  stromu, inak `false` (napr. slovo už v strome existuje a tak nemôže byť vložené druhý krát).
- `bool contains(string word)` - Zistí či sa dané slovo nachádza v strome. Metóda vráti hodnotu `true` ak sa slovo
  nachádza v strome, inak `false`.

### Príklady vstupov / výstupov programu

Ak do prefixového stromu vložíme nasledovné slová:

```cpp
Trie trie;
trie.insert("Milan");
trie.insert("Martin");
trie.insert("Martina");
trie.insert("Eva");
```

Strom bude mať nasledovnú štruktúru (farebne sú označené konce slov)

![trie](/images/task42-trie.png)

Následne pre kontrolu prítomnosti slov:

```cpp
trie.contains("Milan") == true
trie.contains("Pavol") == true
trie.contains("Martina") == true
trie.contains("Martir") == true
```

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

using namespace std;

// Trieda pre uzol Trie
class TrieNode {
private:
    char label;
    bool is_end_of_word;
    unordered_map<char, TrieNode*> children;

public:
    // Konštruktor
    TrieNode(char c) : label(c), is_end_of_word(false) {}

    // Getter pre label
    char get_label() const {
        return label;
    }

    // Setter pre koncovosť
    void set_end_of_word(bool value) {
        is_end_of_word = value;
    }

    // Getter pre koncovosť
    bool is_end() const {
        return is_end_of_word;
    }

    // Získaj potomka pre daný znak
    TrieNode* get_child(char c) const {
        auto it = children.find(c);
        return it != children.end() ? it->second : nullptr;
    }

    // Pridaj dieťa, ak ešte neexistuje
    TrieNode* add_child(char c) {
        if (children.find(c) == children.end()) {
            children[c] = new TrieNode(c);
        }
        return children[c];
    }

    // Prístup k všetkým deťom (pre prípadné rozšírenie)
    const unordered_map<char, TrieNode*>& get_children() const {
        return children;
    }

    // Destruktor – uvoľní všetky deti rekurzívne
    ~TrieNode() {
        for (auto& pair : children) {
            delete pair.second;
        }
    }
};

// Trieda pre samotný Trie strom
class Trie {
private:
    TrieNode* root;

public:
    // Konštruktor
    Trie() {
        root = new TrieNode('\0'); // Koreň nemá platný znak
    }

    // Vloženie slova do Trie
    bool insert(const string& word) {
        TrieNode* current = root;
        for (char c : word) {
            current = current->add_child(c);
        }
        if (current->is_end()) {
            return false; // slovo už existuje
        } else {
            current->set_end_of_word(true);
            return true;
        }
    }

    // Kontrola či slovo existuje
    bool contains(const string& word) const {
        TrieNode* current = root;
        for (char c : word) {
            current = current->get_child(c);
            if (!current) {
                return false;
            }
        }
        return current->is_end();
    }

    // Destruktor – uvoľní celý strom
    ~Trie() {
        delete root;
    }
};

// Demonštrácia použitia
int main() {
    Trie trie;

    cout << boolalpha; // výpis true/false namiesto 1/0

    // Vkladanie slov
    cout << "Insert 'Milan': " << trie.insert("Milan") << endl;
    cout << "Insert 'Martin': " << trie.insert("Martin") << endl;
    cout << "Insert 'Martina': " << trie.insert("Martina") << endl;
    cout << "Insert 'Eva': " << trie.insert("Eva") << endl;

    // Overenie, že slová sú prítomné
    cout << "\nContains 'Milan': " << trie.contains("Milan") << endl;
    cout << "Contains 'Martin': " << trie.contains("Martin") << endl;
    cout << "Contains 'Martina': " << trie.contains("Martina") << endl;
    cout << "Contains 'Eva': " << trie.contains("Eva") << endl;

    // Skúška s neexistujúcim slovom
    cout << "\nContains 'Mar': " << trie.contains("Mar") << endl;

    // Pokus o opätovné vloženie existujúceho slova
    cout << "\nInsert 'Milan' again: " << trie.insert("Milan") << endl;

    return 0;
}
```

## Očakávaný výstup

```cpp
Insert 'Milan': true
Insert 'Martin': true
Insert 'Martina': true
Insert 'Eva': true

Contains 'Milan': true
Contains 'Martin': true
Contains 'Martina': true
Contains 'Eva': true

Contains 'Mar': false

Insert 'Milan' again: false
```

{{< /details >}}
