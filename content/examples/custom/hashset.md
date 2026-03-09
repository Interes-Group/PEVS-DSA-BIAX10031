---
date: '2026-03-09T23:59:06+01:00'
title: 'HashSet'
---

Tento kód je jednoduchá implementácia množiny (HashSet) pre celé čísla v jazyku C++.
Umožňuje:

- vložiť prvok iba raz,
- odstrániť prvok,
- zistiť, či sa prvok nachádza v množine,
- zistiť počet prvkov,
- a vypísať vnútorné rozloženie prvkov do bucket-ov.
-

Používa sa tu princíp **hašovania**: číslo sa pomocou hašovacej funkcie priradí do konkrétneho „koša“ (bucket).
Ak do rovnakého koša spadne viac hodnôt, ukladajú sa do poľa v danom koši. Toto sa volá **riešenie kolízií pomocou
chainingu**.

```cpp
#include <iostream>
#include <climits>
#include <vector>

using namespace std;

class HashSet {
private:
    vector<vector<int> > buckets;
    int capacity;
    int elementCount;

    int hash(int key) const {
        return key % capacity;
    }

public:
    HashSet(int cap = 10) {
        capacity = cap;
        elementCount = 0;
        buckets.resize(capacity);
    }

    bool insert(int value) {
        int h = hash(value);
        for (int x: buckets[h]) {
            if (x == value) return false;
        }
        buckets[h].push_back(value);
        elementCount++;
        return true;
    }

    int remove(int value) {
        int h = hash(value);
        for (int i = 0; i < buckets[h].size(); i++) {
            if (buckets[h][i] == value) {
                buckets[h][i] = buckets[h].back();
                buckets[h].pop_back();
                elementCount--;
                return value;
            }
        }
        return INT_MIN;
    }

    bool contains(int value) {
        int h = hash(value);
        for (int x: buckets[h]) {
            if (x == value) return true;
        }
        return false;
    }

    int size() const {
        return elementCount;
    }

    string to_string() {
        string result;
        for (int i = 0; i < buckets.size(); i++) {
            result += std::to_string(i) + " : ";
            for (int x: buckets[i]) {
                result += std::to_string(x) + ", ";
            }
            result += "\n";
        }
        return result;
    }
};

int main() {
    HashSet hs(3);
    bool insertSuccess = hs.insert(1);
    insertSuccess = hs.insert(2);
    insertSuccess = hs.insert(4);
    insertSuccess = hs.insert(3);
    insertSuccess = hs.insert(5);
    insertSuccess = hs.insert(5);
    insertSuccess = hs.insert(2);

    cout << hs.to_string() << endl;

    hs.remove(2);
    cout << hs.to_string() << endl;

    hs.remove(1);
    cout << hs.to_string() << endl;

    cout << "Contains 2? " << (hs.contains(2) ? "true" : "false") << endl;

    return 0;
}
```

# Vysvetlenie

## 1. Definícia triedy `HashSet`

```c++
class HashSet {
```

Trieda predstavuje vlastnú dátovú štruktúru – množinu čísel.

Množina má tieto vlastnosti:

- prvok sa v nej nemá opakovať,
- poradie prvkov nie je podstatné,
- dôležité sú rýchle operácie `insert`, `remove`, `contains`.

---

## 2. Súkromné premenné triedy

```c++
private:
    vector<vector<int> > buckets;
    int capacity;
    int elementCount;
```

### `buckets`

- Je to **vektor vektorov celých čísel**.
- Každá pozícia predstavuje jeden „kôš“.
- Do každého koša sa ukladajú čísla, ktoré majú rovnaký výsledok hašovania.

Príklad:

- ak `capacity = 3`,
- tak číslo `4` ide do koša `4 % 3 = 1`,
- číslo `1` ide tiež do koša `1 % 3 = 1`.

Teda v koši na indexe `1` môžu byť napríklad hodnoty `1` a `4`.

### `capacity`

- Počet košov.
- Určuje rozsah indexov od `0` po `capacity - 1`.

### `elementCount`

- Počíta, koľko prvkov je aktuálne v množine.

---

## 3. Hašovacia funkcia

```c++
int hash(int key) const {
    return key % capacity;
}
```

Táto funkcia vypočíta, do ktorého koša sa má číslo uložiť.

### Ako funguje:

Použije sa zvyšok po delení:

```c++
key % capacity
```

Ak je napríklad:

- `capacity = 3`
- `key = 5`

tak:

```c++
5 % 3 = 2
```

Hodnota `5` teda patrí do koša s indexom `2`.

### Prečo je dôležitá:

Hašovacia funkcia umožňuje rýchlo nájsť miesto, kde by sa prvok mal nachádzať.

### Poznámka:

Táto verzia je jednoduchá a vhodná na učenie.  
Pri záporných číslach by mohla vracať záporný index, čo by bol problém. Pre kladné čísla funguje správne.

---

## 4. Konštruktor

```c++
HashSet(int cap = 10) {
    capacity = cap;
    elementCount = 0;
    buckets.resize(capacity);
}
```

Konštruktor sa zavolá pri vytvorení objektu triedy.

### Čo robí:

- nastaví počet košov na hodnotu `cap`,
- nastaví počet prvkov na `0`,
- vytvorí potrebný počet prázdnych košov.

### Predvolená hodnota:

```c++
int cap = 10
```

Ak používateľ nezadá kapacitu, automaticky sa použije `10`.

Príklad:

```c++
HashSet a;
```

→ kapacita bude 10

```c++
HashSet b(3);
```

→ kapacita bude 3

---

## 5. Funkcia `insert`

```c++
bool insert(int value) {
    int h = hash(value);
    for (int x: buckets[h]) {
        if (x == value) return false;
    }
    buckets[h].push_back(value);
    elementCount++;
    return true;
}
```

Táto funkcia vloží prvok do množiny.

### Krok za krokom:

1. Vypočíta sa index koša:

```c++
int h = hash(value);
```

2. Prejde sa celý daný kôš:

```c++
for (int x: buckets[h])
```

a skontroluje sa, či sa hodnota už nenachádza v množine.

3. Ak sa hodnota nájde:

```c++
if (x == value) return false;
```

vloženie zlyhá, pretože množina nemá obsahovať duplicity.

4. Ak sa hodnota nenašla:

```c++
buckets[h].push_back(value);
   elementCount++;
   return true;
```

hodnota sa vloží do koša a zvýši sa počet prvkov.

### Návratová hodnota:

- `true` – prvok sa úspešne vložil
- `false` – prvok už v množine existoval

---

## 6. Funkcia `remove`

```c++
int remove(int value) {
    int h = hash(value);
    for (int i = 0; i < buckets[h].size(); i++) {
        if (buckets[h][i] == value) {
            buckets[h][i] = buckets[h].back();
            buckets[h].pop_back();
            elementCount--;
            return value;
        }
    }
    return INT_MIN;
}
```

Táto funkcia odstráni prvok z množiny.

### Postup:

1. Nájde sa správny kôš:

```c++
int h = hash(value);
```

2. Prechádza sa prvok po prvku v danom koši:

```c++
for (int i = 0; i < buckets[h].size(); i++)
```

3. Ak sa nájde zhodná hodnota:

```c++
if (buckets[h][i] == value)
```

4. Odstránenie sa spraví zaujímavým spôsobom:

```c++
buckets[h][i] = buckets[h].back();
   buckets[h].pop_back();
```

- na miesto mazaného prvku sa dá posledný prvok z koša,
- potom sa posledný prvok odstráni.

### Prečo je to výhodné:

Mazanie z konca `vector` je rýchle.  
Netreba posúvať všetky ďalšie prvky doľava.

### Nevýhoda:

Poradie prvkov v koši sa zmení.  
To však pri množine nevadí, lebo poradie nie je dôležité.

### Návratová hodnota:

- ak sa prvok našiel a odstránil → vráti sa jeho hodnota,
- ak sa nenašiel → vráti sa `INT_MIN`.

To je použitý „signál“, že mazanie nebolo úspešné.

---

## 7. Funkcia `contains`

```c++
bool contains(int value) {
    int h = hash(value);
    for (int x: buckets[h]) {
        if (x == value) return true;
    }
    return false;
}
```

Táto funkcia zisťuje, či sa hodnota v množine nachádza.

### Ako funguje:

- vypočíta správny kôš,
- prejde všetky prvky v tomto koši,
- ak nájde zhodu, vráti `true`,
- inak `false`.

### Výhoda:

Nie je potrebné prehľadávať všetky koše, iba jeden konkrétny.

---

## 8. Funkcia `size`

```c++
int size() const {
    return elementCount;
}
```

Vracia počet prvkov v množine.

### Prečo je `const`:

Znamená to, že funkcia nemení stav objektu.  
Len číta hodnotu `elementCount`.

---

## 9. Funkcia `to_string`

```c++
string to_string() {
    string result;
    for (int i = 0; i < buckets.size(); i++) {
        result += std::to_string(i) + " : ";
        for (int x: buckets[i]) {
            result += std::to_string(x) + ", ";
        }
        result += "\n";
    }
    return result;
}
```

Táto funkcia vytvorí textový výpis všetkých košov.

### Čo robí:

- pre každý kôš vypíše jeho index,
- potom vypíše všetky hodnoty uložené v danom koši,
- nakoniec pridá nový riadok.

### Príklad výstupu:

```c++
0 : 3,
1 : 1, 4,
2 : 2, 5,
```

To pomáha pochopiť, ako sa prvky rozdelili podľa hašovacej funkcie.

### Poznámka:

Funkcia sa volá `to_string`, čo je v poriadku, ale názvom sa podobá na `std::to_string`.  
V tomto kóde to nevadí, lebo sa pri prevode čísel používa explicitne `std::to_string(...)`.

---

## 10. Funkcia `main`

```c++
int main() {
    HashSet hs(3);
```

Tu sa vytvorí objekt `HashSet` s kapacitou `3`, teda s tromi košmi.

---

### Vkladanie prvkov

```c++
bool insertSuccess = hs.insert(1);
    insertSuccess = hs.insert(2);
    insertSuccess = hs.insert(4);
    insertSuccess = hs.insert(3);
    insertSuccess = hs.insert(5);
    insertSuccess = hs.insert(5);
    insertSuccess = hs.insert(2);
```

Do množiny sa postupne skúšajú vložiť čísla:

- `1`
- `2`
- `4`
- `3`
- `5`
- `5` znovu
- `2` znovu

### Dôležité:

Druhé vloženie `5` a druhé vloženie `2` zlyhá, pretože tieto hodnoty už v množine sú.

Premenná `insertSuccess` síce zachytáva výsledok, ale ďalej sa nepoužíva.  
To znamená, že v tomto príklade slúži skôr len ako ukážka, že `insert` vracia `bool`.

---

### Prvý výpis

```c++
cout << hs.to_string() << endl;
```

Vypíše sa obsah všetkých košov po vložení prvkov.

Pri kapacite `3` bude rozdelenie vyzerať takto:

- `1 % 3 = 1`
- `2 % 3 = 2`
- `4 % 3 = 1`
- `3 % 3 = 0`
- `5 % 3 = 2`

Takže približne:

- kôš `0`: `3`
- kôš `1`: `1, 4`
- kôš `2`: `2, 5`

---

### Odstránenie prvku `2`

```c++
hs.remove(2);
    cout << hs.to_string() << endl;
```

Prvok `2` sa odstráni a potom sa množina znovu vypíše.

Keďže `2` bol v koši `2`, po odstránení tam zostane už len `5`.

---

### Odstránenie prvku `1`

```c++
hs.remove(1);
    cout << hs.to_string() << endl;
```

Odstráni sa `1` a množina sa opäť vypíše.

V koši `1` bol pravdepodobne aj `4`, takže po mazaní môže `4` zostať na jeho mieste.

---

### Kontrola výskytu prvku `2`

```c++
cout << "Contains 2? " << (hs.contains(2) ? "true" : "false") << endl;
```

Tu sa pomocou ternárneho operátora vypíše:

- `"true"` ak sa `2` nachádza v množine,
- `"false"` ak nie.

Keďže `2` už bolo odstránené, výsledok bude:

```c++
Contains 2? false
```

---

### Ukončenie programu

```c++
return 0;
}
```

Program sa korektne ukončí.

---

# 11. Aký princíp dátovej štruktúry sa tu demonštruje

Tento kód ukazuje základnú myšlienku **hash setu**:

1. prvok sa pomocou hašovacej funkcie priradí do koša,
2. v koši sa kontroluje, či už existuje,
3. ak nie, vloží sa,
4. vyhľadávanie aj mazanie prebieha len v príslušnom koši.

To je dôvod, prečo sú hashové štruktúry často rýchle.

# 12. Zhrnutie

Kód implementuje vlastnú triedu `HashSet`, ktorá:

- ukladá **unikátne celé čísla**,
- rozdeľuje ich do košov pomocou operácie `%`,
- rieši kolízie cez `vector<vector<int>>`,
- umožňuje:
    - `insert`
    - `remove`
    - `contains`
    - `size`
    - `to_string`

Je to veľmi dobrá ukážka základov **hašovacích dátových štruktúr** v C++.

Ak chceš, môžem ti ešte napísať aj:

1. **riadok po riadku komentovanú verziu kódu**, alebo
2. **kratšie “študijné poznámky” v bodoch**, ktoré sa hodia napríklad pred skúšku.