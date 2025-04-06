---
date: '2025-04-06T20:05:43+02:00'
title: 'Riešenie Zadania 1'
---

Zadanie sa zameriavalo na návrh a implementáciu dátovej štruktúry pre efektívne ukladanie, vyhľadávanie a manipuláciu s
údajmi. Študenti museli pochopiť, ako funguje hierarchická organizácia dát – najčastejšie pomocou stromovej štruktúry (
napríklad prefixový strom či binárny strom) – a zabezpečiť správnu implementáciu základných operácií ako vkladanie,
vyhľadávanie a mazanie. Kľúčové bolo uvedomiť si, ako riešiť prácu s dynamickou pamäťou, rekurzívnym prechádzaním a
optimalizáciou výkonu, aby výsledné riešenie bolo nielen funkčné, ale aj efektívne a prehľadné.

Študenti sa pri riešení zadania mohli stretnúť s niekoľkými problémami, medzi ktoré patrili napríklad:

- **Práca s dynamickou pamäťou a pointermi:** Správne alokovanie a uvoľňovanie pamäte (napr. implementácia destruktorov)
  je kľúčové, inak hrozia memory leaky alebo segmentation faulty.
- **Rekurzia a prechádzanie stromov:** Implementácia rekurzívnych algoritmov pre vkladanie, vyhľadávanie a mazanie uzlov
  môže byť zložitá, najmä pri riešení hraničných prípadov.
- **Správna manipulácia s dátovými štruktúrami:** Pochopenie, ako funguje hierarchická organizácia dát (napr. prefixový
  strom) a ako správne pracovať s deťmi uzlov cez mapy, môže byť náročné.
- **Práca s case-insensitive operáciami:** Transformácia znakov na malé písmená (alebo veľké) pre správne porovnávanie a
  ukladanie údajov môže viesť k chybám, ak nie je realizovaná konzistentne.
- **Spracovanie viacerých záznamov:** Riešenie situácií, kde je pre jedno priezvisko evidovaných viac krstných mien a
  čísel, si vyžaduje správnu manipuláciu s vnorenými mapami a výber správneho záznamu.

Tieto problémy si vyžadovali dôkladné pochopenie algoritmickej logiky a správnu implementáciu operácií, aby výsledný
telefónny zoznam fungoval efektívne a spoľahlivo.

## Implementácia

Nižšie je uvedená príkladná implementácia zadania. Samozrejme spôsobov vypracovania mohlo byť viacero.

```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <unordered_map>
#include <utility>
#include <regex>

using namespace std;

class TrieNode {
public:
    char label;
    bool isWord = false;
    unordered_map<char, TrieNode *> children;
    unordered_map<string, string> data;

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

class PhoneBook {
public:
    TrieNode *root;
    queue<string> numbers_to_call;

    PhoneBook() {
        root = new TrieNode('\0');
    }

    void insert(string first_name, const string &last_name, string number) const {
        TrieNode *node = root;
        for (char c: last_name) {
            node = node->add_child(c);
        }
        node->isWord = true;
        transform(first_name.begin(), first_name.end(), first_name.begin(), ::tolower);
        node->data[first_name] = std::move(number);
    }

    TrieNode *find(const string &last_name) const {
        TrieNode *node = root;
        for (char c: last_name) {
            node = node->get_child(c);
            if (!node) return nullptr;
        }
        if (node->isWord) return node;
        return nullptr;
    }

    string get(string first_name, const string &last_name) const {
        TrieNode *node = find(last_name);
        if (!node) return "";
        transform(first_name.begin(), first_name.end(), first_name.begin(), ::tolower);
        return node->data[first_name];
    }

    unordered_map<string, string> *get(const string &last_name) const {
        TrieNode *node = find(last_name);
        return &node->data;
    }

    bool remove_number(const string &first_name, const string &last_name, int idx, TrieNode *node) {
        if (!node) return false;
        if (idx == last_name.length() - 1) {
            if (!node->isWord) return false;
            node->data.erase(first_name);
            return node->data.empty();
        }
        if (node->has_child(last_name[idx + 1])) {
            bool result = remove_number(first_name, last_name, idx + 1, node->get_child(last_name[idx + 1]));
            if (result) {
                node->remove_child(last_name[idx + 1]);
                return !node->isWord && node->children.empty();
            }
            return result;
        } else {
            return false;
        }
    }

    void remove(string first_name, string last_name) {
        transform(first_name.begin(), first_name.end(), first_name.begin(), ::tolower);
        transform(last_name.begin(), last_name.end(), last_name.begin(), ::tolower);
        remove_number(first_name, last_name, -1, root);
    }

    void get_names(string prefix, vector<string> &names, TrieNode *node) const {
        prefix = prefix + node->label;
        if (node->isWord) names.push_back(prefix);
        for (auto [key, node]: node->children) {
            get_names(prefix, names, node);
        }
    }

    void names(vector<string> &names) const {
        for (auto [key, node]: root->children) {
            get_names("", names, node);
        }
    }

    void enqueue(const string &first_name, const string &last_name) {
        string number = get(first_name, last_name);
        if (!number.empty()) {
            numbers_to_call.push(first_name + " " + last_name + " - " + number);
        }
    }

    string get_number_to_call() const {
        return numbers_to_call.front();
    }

    string get_next_number_to_call() {
        numbers_to_call.pop();
        return numbers_to_call.front();
    }

    void pop_number() {
        numbers_to_call.pop();
    }

    ~PhoneBook() {
        delete root;
    }
};

void print(PhoneBook *book) {
    vector<string> names;
    book->names(names);
    for (string name: names) {
        cout << name << " ";
    }
    cout << endl;
}

string capitalize(string str) {
    transform(str.begin(), str.end(), str.begin(), ::tolower);
    str[0] = toupper(str[0]);
    return str;
}

unordered_map<string, string>::iterator get_by_index(unordered_map<string, string> *data, int choice) {
    int i = 0;
    i = 0;
    for (auto it = data->begin(); it != data->end(); it++) {
        if (i == choice) {
            return it;
        }
        i++;
    }
    return data->end();
}

string ltrim(const string &s) {
    return regex_replace(s, regex("^\\s+"), string(""));
}

string rtrim(const string &s) {
    return regex_replace(s, regex("\\s+$"), string(""));
}

string trim(const string &s) {
    return ltrim(rtrim(s));
}

string get_input(const string &message) {
    string input;
    while (true) {
        cout << message << ": ";
        cin >> input;
        input = trim(input);
        if (!input.empty()) break;
    }
    return input;
}

/**
 * Funkcia pre zabezpečenie vyhľadania čísla na základe používateľského vstupu
 * @param book telefónny zoznam
 * @return vektor kde na indexoch 0-> krstné meno, 1-> priezvisko, 2-> telefonne čislo
 */
vector<string> search(PhoneBook *book) {
    vector<string> entry;
    string lastname_input = get_input("Zadaj priezvisko");
    unordered_map<string, string> *data = book->get(lastname_input);
    if (data->empty()) {
        cout << "Pre zadané meno '" << lastname_input << "' neexistuje záznam" << endl;
    }
    cout << "Pre zadané priezvisko sú evidované tieto krstné mená: " << endl;;
    int i = 0;
    for (auto it = data->begin(); it != data->end(); it++) {
        cout << i << " " << it->first << endl;
        i++;
    }
    int firstname_choice;
    cout << "> ";
    cin >> firstname_choice;
    auto data_entry = get_by_index(data, firstname_choice);
    if (data_entry == data->end()) {
        cout << "Bola zadaná neznáma voľba" << endl;
        return entry;
    }
    entry.push_back(capitalize(data_entry->first));
    entry.push_back(capitalize(lastname_input));
    entry.push_back(data_entry->second);
    return entry;
}

int main() {
    PhoneBook phone_book;
    phone_book.insert("Emil", "fog", "0914777666");
    phone_book.insert("Henry", "Smart", "0915745734");
    phone_book.insert("John", "Smith", "0907566123");
    phone_book.insert("Sant", "Fog", "0905333444");

    cout << "Vitajte v jednoduchom telefonnom zozname" << endl;

    string input;
    while (true) {
        cout << "----------------------------------------" << endl;
        cout << "1. Pridaj číslo" << endl;
        cout << "2. Nájdi číslo" << endl;
        cout << "3. Vymaž číslo" << endl;
        cout << "4. Pridaj číslo do telefonickej fronty" << endl;
        cout << "5. Začni telefonickú frontu" << endl;
        cout << "6. Vypíš mená" << endl;
        cout << "Pre ukončenie napíšte 'Q'" << endl;

        cout << "> ";
        cin >> input;
        if (input == "Q" || input == "q") break;
        int choice = strtol(input.c_str(), nullptr, 10);
        if (choice == 1) {
            string first = get_input("Zadaj krstné meno");
            string last = get_input("Zadaj priezvisko");
            string number = get_input("Zadaj telefónne čislo");
            phone_book.insert(first, last, number);
            cout << "Nový záznam úspešne vložený" << endl;
        }
        if (choice == 2) {
            vector<string> found = search(&phone_book);
            if (!found.empty()) {
                cout << found[0] + " " + found[1] + " - " + found[2] << endl;
            }
        }
        if (choice == 3) {
            vector<string> found = search(&phone_book);
            if (!found.empty()) {
                char delete_choice;
                cout << found[0] + " " + found[1] + " - " + found[2] << endl;
                cout << "Vymazať záznam (a/n): ";
                cin >> delete_choice;
                if (delete_choice == 'a') {
                    phone_book.remove(found[0], found[1]);
                    cout << "Záznam úspešne vymazaný";
                }
            }
        }
        if (choice == 4) {
            vector<string> found = search(&phone_book);
            if (!found.empty()) {
                char enqueue_choice;
                cout << found[0] + " " + found[1] + " - " + found[2] << endl;
                cout << "Pridať číslo do fronty (a/n): ";
                cin >> enqueue_choice;
                if (enqueue_choice == 'a') {
                    phone_book.enqueue(found[0], found[1]);
                    cout << "Číslo pridané do fronty" << endl;
                }
            }
        }
        if (choice == 5) {
            string entry = phone_book.get_number_to_call();
            if (entry.empty()) {
                cout << "Telefonická fronta je prázdna" << endl;
            } else {
                bool force_end;
                while (!entry.empty()) {
                    cout << entry << endl;
                    cout << "Ďalšie číslo (a/n), alebo chcete ukončiť prechádzanie fronty (k): ";
                    char call_choice;
                    cin >> call_choice;
                    if (call_choice == 'k') {
                        phone_book.pop_number();
                        force_end = true;
                        break;
                    }
                    if (call_choice == 'a') {
                        entry = phone_book.get_next_number_to_call();
                    }
                }
                if (!force_end) cout << "Fronta už neobsahuje žiadne ďalšie čísla" << endl;
            }
        }
        if (choice == 6) {
            print(&phone_book);
        }
        cout << endl;
    }

    cout << "Dovidenia" << endl;

    return 0;
}
```

## Vysvetlenie implementácie

Tento kód implementuje jednoduchý telefónny zoznam, ktorý využíva prefixový strom (trie) na ukladanie záznamov a zároveň
spravuje frontu telefónnych čísel, ktoré majú byť volané.

### Hlavné časti kódu

#### 1. Trieda TrieNode

- **Účel:** Reprezentuje jeden uzol v prefixovom strome.
- **Atribúty:**
    - **label:** Znak, ktorý uzol predstavuje.
    - **isWord:** Boolean príznak, ktorý indikuje, či cesta do tohto uzla reprezentuje celé (platné) slovo (v našom
      prípade priezvisko).
    - **children:** Hašovacia mapa (unordered_map) s deťmi – pre každý znak (prevádzkovaný na malé písmeno) je uložený
      ukazovateľ na poduzol.
    - **data:** Mapa, kde kľúčom je krstné meno (uložené v malých písmenách) a hodnotou telefónne číslo.
- **Metódy:**
    - `get_child`, `has_child` a `add_child`: Umožňujú vyhľadávať alebo vytvárať potomkov pre daný znak.
    - `remove_child`: Odstráni dieťa z mapy a uvoľní jeho pamäť.
- **Destruktor:** Rekurzívne uvoľňuje pamäť všetkých detských uzlov.

#### 2. Trieda PhoneBook

- **Účel:** Spravuje telefónny zoznam pomocou prefixového stromu, pričom kľúčom pre strom je priezvisko.
- **Hlavné atribúty:**
    - **root:** Koreňový uzol stromu, inicializovaný s prázdnym znakom.
    - **numbers_to_call:** Fronta (queue) reťazcov, kde každý reťazec predstavuje záznam (krstné meno, priezvisko a
      telefónne číslo), ktorý sa má následne volať.
- **Metódy:**
    - **insert:** Prechádza strom pomocou znakov priezviska; na konci uzla označí, že sa jedná o koniec slova, a uloží
      do `data` telefónne číslo pod kľúčom (krstné meno v malých písmenách).
    - **find & get:** Vyhľadávajú uzol pre dané priezvisko a potom v ňom nájdu telefónne číslo podľa krstného mena.
      Metóda `get` je pre získanie jedného čísla, ale existuje aj verzia, ktorá vráti celú mapu záznamov (pre prípady,
      keď je pod jedným priezviskom viac mien).
    - **remove:** Implementovaná rekurzívna funkcia `remove_number`, ktorá prejde strom podľa priezviska, odstráni
      zodpovedajúci záznam (podľa krstného mena) a prípadne aj zbytočné uzly, ak už neobsahujú žiadne dáta.
    - **names:** Rekurzívne prechádza celý strom a zbiera všetky záznamy (priezviská) do zoznamu.
    - **enqueue & front operácie:** Funkcie `enqueue`, `get_number_to_call`, `get_next_number_to_call` a `pop_number`
      slúžia na správu fronty čísel na volanie. Pri pridaní do fronty sa vyhľadá telefónne číslo podľa krstného a
      priezviska a vytvorí sa reťazec vo formáte „krstné priezvisko - číslo“.

#### 3. Pomocné funkcie

- **Input a formátovanie:**
    - `get_input`: Zabezpečí zadanie vstupu od používateľa a orezanie prázdnych medzier (pomocou funkcií `ltrim`,
      `rtrim` a `trim`, ktoré využívajú regulárne výrazy).
    - `capitalize`: Premení prvé písmeno reťazca na veľké a zvyšok ponechá malými.
- **get_by_index:** Pre iteráciu cez mapu záznamov podľa indexu – používa sa v procese výberu správneho krstného mena,
  ak je pre dané priezvisko evidovaných viac mien.

#### 4. Hlavný program (funkcia main)

- **Inicializácia:** Vytvorí sa inštancia triedy `PhoneBook` a vložia sa niektoré preddefinované záznamy (napríklad "
  Emil Fog", "Henry Smart", "John Smith", "Sant Fog").
- **Interaktívne menu:**  
  Používateľ má na výber z niekoľkých možností:
    1. **Pridaj číslo:** Získajú sa krstné meno, priezvisko a telefónne číslo, ktoré sa vložia do stromu.
    2. **Nájdi číslo:** Na základe priezviska sa vyhľadajú všetky krstné mená a používateľ vyberie požadovaný záznam;
       následne sa zobrazí telefónne číslo.
    3. **Vymaž číslo:** Po vyhľadaní sa používateľovi ponúkne možnosť odstrániť záznam.
    4. **Pridaj číslo do telefonickej fronty:** Vybraný záznam sa vloží do fronty, ktorú spravuje telefónny zoznam.
    5. **Začni telefonickú frontu:** Používateľ prechádza frontou, kde sa jednotlivé záznamy postupne zobrazujú a volá
       sa ich ďalší (prípadne sa fronta modifikuje).
    6. **Vypíš mená:** Vypíšu sa všetky záznamy v telefónnom zozname (napr. priezviská podľa stromovej štruktúry).
- **Ukončenie:** Program beží v slučke, kým používateľ nezadá príkaz „Q“.

### Diagram – Uloženie záznamu v Trie

Predstavme si, že vložíme záznamy pre priezvisko „fog“:

```
         (root)
            |
            f
            |
            o
            |
            g  (isWord = true)
          data: { "emil"  -> "0914777666",
                  "sant"  -> "0905333444" }
```

- **Navigácia stromom:** Pri vkladaní alebo vyhľadávaní sa prechádza znaky priezviska ("f" → "o" → "g").
- **Data map:** Na konci (uzol s "g") je uložená mapa, kde sú uložené telefónne čísla pre rôzne krstné mená (vždy
  uložené v malých písmenách).

### Zhrnutie

- **Prefixový strom (Trie):** Efektívne ukladá priezviská, pričom zdieľa spoločné prefixy.
- **Mapovanie údajov:** Na úrovni koncových uzlov sa uchovávajú záznamy (krstné mená a telefónne čísla).
- **Správa fronty:** Implementovaná pomocou STL fronty, ktorá umožňuje spracovanie zoznamu čísel na volanie.
- **Interaktívne funkcie:** Program umožňuje pridávať, vyhľadávať, mazať záznamy a spravovať telefonickú frontu
  prostredníctvom jednoduchého menu a vstupov od používateľa.

Tento kód teda spája dátovú štruktúru (prefixový strom) s praktickou aplikáciou – správou telefónneho zoznamu a fronty
hovorov, pričom využíva aj pomocné funkcie na formátovanie vstupu a výstupu.
