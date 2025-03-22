---
date: '2025-03-21T23:13:41+01:00'
title: 'HashMap'
---

Príklad implementácie štruktúry **hash** mapy. Táto implementácia ukladá len hodnoty typu `int`.
Jednotlivé hodnoty sú uložené do "vedier" (z angl. bucket), alebo chlievikov, pre hodnoty, ktorým je vypočítaný rovnaký
**hash**.
Prístup cez vypočítaný hash zrýchli prístup na _O(1)+O(n)_ kde _n_ je dĺžka najväčšieho bucketu. Čo je lepšie ako
klasický zoznam.

```cpp
#include <iostream>
#include <vector>

using namespace std;

class PrvokZoznamu {
public:
    int hodnota;
    PrvokZoznamu *dalsi = nullptr;
    PrvokZoznamu *predchadzajuci = nullptr;

    /**
     * @brief Konštruktor pre vytvorenie nového prvku so zadanou hodnotou.
     * @param hodnota Hodnota, ktorú prvok bude obsahovať.
     */
    explicit PrvokZoznamu(int hodnota) : hodnota(hodnota) {
    }

    /**
     * @brief Vytlačí hodnoty všetkých prvkov v zozname, počnúc týmto prvkom.
     */
    void print() {
        cout << hodnota << ", ";
        if (dalsi) {
            dalsi->print();
        }
    }

    /**
     * @brief Vyhľadá prvok so zadanou hodnotou v zozname.
     * @param najdi Hľadaná hodnota.
     * @return Ukazateľ na prvý nájdený prvok alebo nullptr, ak sa nenájde.
     */
    PrvokZoznamu *find(int najdi) {
        if (hodnota == najdi) return this;
        if (dalsi) return dalsi->find(najdi);
        return nullptr;
    }

    /**
     * @brief Získa posledný prvok v zozname.
     * @return Ukazateľ na posledný prvok.
     */
    PrvokZoznamu *last() {
        if (!dalsi) return this;
        else return dalsi->last();
    }

    /**
     * @brief Získa prvý prvok v zozname.
     * @return Ukazateľ na prvý prvok.
     */
    PrvokZoznamu *first() {
        if (!predchadzajuci) return this;
        else return predchadzajuci->first();
    }

    /**
     * @brief Vloží nový prvok so zadanou hodnotou na koniec zoznamu, ak už neexistuje.
     * @param nova_hodnota Hodnota nového prvku.
     * @return Ukazateľ na novo vložený prvok alebo nullptr, ak už existuje.
     */
    PrvokZoznamu *insert(int nova_hodnota) {
        PrvokZoznamu *obsahuje_hodnotu = find(nova_hodnota);
        if (obsahuje_hodnotu) return nullptr;
        PrvokZoznamu *posledny = last();
        PrvokZoznamu *vkladany = new PrvokZoznamu(nova_hodnota);
        posledny->dalsi = vkladany;
        vkladany->predchadzajuci = posledny;
        return vkladany;
    }

    /**
     * @brief Odstráni prvok so zadanou hodnotou zo zoznamu.
     * @param na_vymazanie Hodnota prvku, ktorý sa má odstrániť.
     * @return Ukazateľ na nový začiatok zoznamu po odstránení.
     */
    PrvokZoznamu *remove(int na_vymazanie) {
        if (na_vymazanie == hodnota) {
            if (predchadzajuci) {
                predchadzajuci->dalsi = this->dalsi;
            }
            if (dalsi) {
                dalsi->predchadzajuci = predchadzajuci;
            }
            PrvokZoznamu *prvy = first();
            if (prvy == this) {
                prvy = prvy->dalsi;
                delete this;
            }
            return prvy;
        }
        if (dalsi) dalsi->remove(na_vymazanie);
    }

    /**
     * @brief Vymaže celý zoznam, počnúc posledným prvkom a idúc späť k prvému.
     */
    void clear() {
        PrvokZoznamu *posledny = last();
        while (posledny) {
            if (!posledny->predchadzajuci) {
                delete posledny;
                break;
            }
            posledny = posledny->predchadzajuci;
            delete posledny->dalsi;
        }
    }
};

class HashMap {
public:
    vector<PrvokZoznamu *> buckets;
    int kapacita;

    /**
     * @brief Konštruktor pre vytvorenie hash mapy so zadanou kapacitou.
     * @param kapacita Počet "vedier" hash mapy.
     */
    explicit HashMap(int kapacita) : kapacita(kapacita) {
        for (int i = 0; i < kapacita; i++) {
            buckets.push_back(nullptr);
        }
    }

    /**
     * @brief Vypočíta hash pre danú hodnotu.
     * @param hodnota Hodnota, pre ktorú sa má vypočítať hash.
     * @return Výsledný hash ako index do poľa.
     */
    int calc_hash(int hodnota) const {
        return hodnota % kapacita;
    }

    /**
     * @brief Vytlačí obsah celej hash mapy.
     */
    void print() {
        for (int i = 0; i < buckets.size(); i++) {
            cout << "[" << i << "]" << " : ";
            if (buckets[i]) buckets[i]->print();
            else cout << "NULL";
            cout << endl;
        }
    }

    /**
     * @brief Vloží hodnotu do hash mapy, ak ešte neexistuje.
     * @param hodnota Hodnota, ktorá sa má vložiť.
     * @return true, ak bola hodnota vložená; false, ak už existovala.
     */
    bool insert(int hodnota) {
        const int hash = calc_hash(hodnota);
        if (!buckets[hash]) {
            buckets[hash] = new PrvokZoznamu(hodnota);
            return true;
        } else {
            PrvokZoznamu *zoznam = buckets[hash];
            PrvokZoznamu *vlozeny = zoznam->insert(hodnota);
            return vlozeny != nullptr;
        }
    }

    /**
     * @brief Skontroluje, či daná hodnota existuje v hash mape.
     * @param hodnota Hľadaná hodnota.
     * @return Index vedra, v ktorom sa hodnota nachádza; -1 ak neexistuje.
     */
    int contains(int hodnota) {
        const int hash = calc_hash(hodnota);
        if (!buckets[hash]) return -1;
        PrvokZoznamu *najdeny = buckets[hash]->find(hodnota);
        return najdeny != nullptr ? hash : -1;
    }

    /**
     * @brief Odstráni danú hodnotu z hash mapy.
     * @param hodnota Hodnota, ktorá sa má odstrániť.
     */
    void remove(int hodnota) {
        const int hash = calc_hash(hodnota);
        if (!buckets[hash]) return;
        PrvokZoznamu *novy_prvy = buckets[hash]->remove(hodnota);
        buckets[hash] = novy_prvy;
    }

    /**
     * @brief Dekštruktor, ktorý uvoľní všetky prvky hash mapy.
     */
    ~HashMap() {
        for (PrvokZoznamu *bucket: buckets) {
            bucket->clear();
        }
    }
};
```

# Vysvetlenie

## Trieda `PrvokZoznamu`

Predstavuje **prvok obojstranného zoznamu** (`doubly linked list`), ktorý sa používa v každom "vedre" hash mapy.

### Atribúty:

- `int hodnota` – hodnota, ktorú prvok reprezentuje
- `PrvokZoznamu *dalsi` – ukazateľ na nasledujúci prvok
- `PrvokZoznamu *predchadzajuci` – ukazateľ na predchádzajúci prvok

### Metódy:

#### `explicit PrvokZoznamu(int hodnota)`

- Konštruktor, ktorý nastaví hodnotu prvku.

#### `void print()`

- Rekurzívne vypisuje všetky hodnoty od tohto prvku ďalej.

#### `PrvokZoznamu* find(int najdi)`

- Hľadá v zozname prvok s danou hodnotou.
- Ak nájde, vráti pointer, inak `nullptr`.

#### `PrvokZoznamu* last()`

- Vráti posledný prvok v zozname (prechádza cez `dalsi`).

#### `PrvokZoznamu* first()`

- Vráti prvý prvok v zozname (prechádza späť cez `predchadzajuci`).

#### `PrvokZoznamu* insert(int nova_hodnota)`

- Najprv overí, či hodnota už neexistuje (`find()`).
- Ak nie, vytvorí nový prvok a vloží ho na koniec zoznamu.
- Vráti pointer na nový prvok alebo `nullptr`, ak už existuje.

#### `PrvokZoznamu* remove(int na_vymazanie)`

- Odstráni prvok so zadanou hodnotou.
- Upraví odkazy `predchadzajuci` a `dalsi`, aby bol zoznam stále súvislý.
- Ak sa odstraňuje prvý prvok, vráti nový začiatok zoznamu.
- Nevracia nič pri zlyhaní, čo môže byť nebezpečné (viď nižšie).

#### `void clear()`

- Vymaže celý zoznam od konca smerom k začiatku, bezpečne uvoľní pamäť.

## Trieda `HashMap`

Trieda, ktorá simuluje **jednoduchú hash mapu**.

### Atribúty:

- `vector<PrvokZoznamu*> buckets` – pole "vedier", v ktorých sú ukazatele na zoznamy (kvôli kolíziám).
- `int kapacita` – počet vedier, ktoré určujú, ako dobre hash rozdeľuje dáta.

### Metódy:

#### `explicit HashMap(int kapacita)`

- Vytvorí hash mapu s daným počtom prázdnych vedier (inicializovaných na `nullptr`).

#### `int calc_hash(int hodnota)`

- Jednoduchá hashovacia funkcia: `hodnota % kapacita`
- Vracia index vedra, do ktorého patrí hodnota.

#### `void print()`

- Vypíše všetky vedrá a ich obsahy (volá `print()` na `PrvokZoznamu`).

#### `bool insert(int hodnota)`

- Vloží novú hodnotu do správneho vedra (podľa hash).
- Ak vedro ešte neexistuje, vytvorí nový prvok.
- Ak existuje, volá `insert()` na zozname.
- Vráti `true` ak sa hodnota vložila, inak `false`.

#### `int contains(int hodnota)`

- Skontroluje, či hodnota existuje v danom vedre (pomocou `find()`).
- Ak sa nájde, vráti index vedra, inak `-1`.

#### `void remove(int hodnota)`

- Odstráni prvok s danou hodnotou.
- Ak po odstránení je prvý prvok iný, aktualizuje vedro.

#### `~HashMap()`

- Destruktor, ktorý volá `clear()` pre každý zoznam vo vedrách a uvoľní pamäť.


