---
date: '2025-03-21T23:13:41+01:00'
title: 'HashMap'
---

Tento program demonštruje jednoduchú vlastnú implementáciu hash mapy v C++. Dátovej štruktúry, ktorá ukladá
dvojice kľúč → hodnota a umožňuje rýchle operácie ako vloženie, vyhľadanie, aktualizáciu a odstránenie.

Účelom je demonštrovať, ako funguje hashovanie a riešenie kolízií pomocou „bucketov“.

```cpp
#include <iostream>
#include <vector>

using namespace std;

class MapEntry {
public:
    string key;
    string value;

    MapEntry(string key, string value) {
        this->key = key;
        this->value = value;
    }
};

class HashMap {
private:
    vector<vector<MapEntry>> buckets;
    int capacity;
    int elementCount;

    int hash(const string &key) const {
        int sum = 0;
        for (char c: key) {
            sum = sum + c;
        }
        return sum % capacity;
    }

public:
    HashMap(int capacity = 10) {
        this->capacity = capacity;
        elementCount = 0;
        buckets.resize(capacity);
    }

    // insert
    void put(const string &key, const string &value) {
        int h = hash(key);
        for (MapEntry &entry: buckets[h]) {
            if (entry.key == key) {
                entry.value = value;
                return;
            }
        }
        buckets[h].push_back(MapEntry(key, value));
        elementCount++;
    }

    // remove
    void remove(const string &key) {
        int h = hash(key);
        for (int i = 0; i < buckets[h].size(); i++) {
            if (buckets[h][i].key == key) {
                buckets[h][i] = buckets[h][buckets[h].size() - 1]; // buckets[h].back()
                buckets[h].pop_back();
                elementCount--;
                return;
            }
        }
    }

    // contains key
    bool containsKey(const string &key) {
        int h = hash(key);
        for (MapEntry &entry: buckets[h]) {
            if (entry.key == key) return true;
        }
        return false;
    }

    // get value
    string get(const string &key) {
        int h = hash(key);
        for (MapEntry &entry: buckets[h]) {
            if (entry.key == key) return entry.value;
        }
        throw runtime_error("No such key");
    }

    int size() const {
        return elementCount;
    }

    string to_string() {
        string r;
        for (int i = 0; i < buckets.size(); i++) {
            r += std::to_string(i) + " : ";
            for (MapEntry &entry: buckets[i]) {
                r += "[" + entry.key + "]" + entry.value + "; ";
            }
            r += "\n";
        }
        return r;
    }
};

int main() {
    HashMap hm(5);
    hm.put("a", "foo");
    hm.put("b", "bar");
    hm.put("ab", "baaaz");
    hm.put("ba", "baz");

    cout << hm.to_string() << endl;

    hm.put("ba", "nova hodnota");

    cout << hm.to_string() << endl;

    hm.remove("a");

    cout << hm.to_string() << endl;


    return 0;
}
```

# Vysvetlenie

## 1. Trieda `MapEntry`

```c++
class MapEntry {
public:
    string key;
    string value;

    MapEntry(string key, string value) {
        this->key = key;
        this->value = value;
    }
};
```

Táto trieda predstavuje **jednu položku v mape**, teda jednu dvojicu:

- `key` = kľúč
- `value` = hodnota

### Čo obsahuje:

- `string key;`  
  uchováva kľúč, podľa ktorého budeme vyhľadávať
- `string value;`  
  uchováva hodnotu priradenú ku kľúču

### Konštruktor

```c++
MapEntry(string key, string value) {
    this->key = key;
    this->value = value;
}
```

Konštruktor nastaví počiatočný kľúč a hodnotu objektu.

- `this->key` znamená: členská premenná objektu
- pravá `key` je parameter konštruktora

Inými slovami:

- do objektu sa uloží zadaný kľúč
- do objektu sa uloží zadaná hodnota

---

## 2. Trieda `HashMap`

```c++
class HashMap {
private:
    vector<vector<MapEntry> > buckets;
    int capacity;
    int elementCount;
```

Toto je hlavná trieda programu — vlastná implementácia hash mapy.

---

### 2.1 Premenné triedy

#### `vector<vector<MapEntry>> buckets;`

Toto je najdôležitejšia časť implementácie.

- vonkajší `vector` predstavuje pole priehradiek, teda **bucketov**
- každý bucket je vnútorný `vector<MapEntry>`
- v každom buckete môže byť viacero položiek

Prečo viacero?  
Pretože rôzne kľúče môžu mať **rovnaký hash index**. Tomu sa hovorí **kolízia**.

Takže:

- hash funkcia určí index bucketu
- do daného bucketu sa vloží položka
- ak tam už niečo je, nová položka sa tam len pridá

Tento spôsob riešenia kolízií sa volá **separate chaining**.

---

#### `int capacity;`

Udáva počet bucketov.

Ak je `capacity = 5`, tak existujú buckety s indexami:

- 0
- 1
- 2
- 3
- 4

---

#### `int elementCount;`

Počíta počet skutočne uložených prvkov v mape.

To znamená:

- pri vložení novej položky sa zvýši
- pri odstránení sa zníži

---

## 3. Hash funkcia

```c++
int hash(string key) {
    int sum = 0;
    for (char c: key) {
        sum = sum + c;
    }
    return sum % capacity;
}
```

Táto funkcia premení reťazec `key` na číslo — index bucketu.

### Ako funguje:

1. nastaví `sum = 0`
2. prejde všetky znaky v reťazci
3. ku `sum` pripočíta ASCII hodnotu každého znaku
4. výsledok vezme modulo `capacity`

### Príklad:

Ak `capacity = 5` a kľúč je `"ab"`:

- `'a'` má ASCII hodnotu 97
- `'b'` má ASCII hodnotu 98
- súčet = 195
- `195 % 5 = 0`

Takže `"ab"` pôjde do bucketu `0`.

---

### Dôležitá poznámka

Táto hash funkcia je **veľmi jednoduchá** a vhodná na výučbu, ale nie je ideálna pre reálne použitie.

Napríklad:

- `"ab"` a `"ba"` dajú rovnaký súčet
- teda skončia v tom istom buckete

To je síce zámerne jednoduché na pochopenie, ale v praxi by sa používala kvalitnejšia hash funkcia.

---

## 4. Konštruktor `HashMap`

```c++
HashMap(int capacity = 10) {
    this->capacity = capacity;
    elementCount = 0;
    buckets.resize(capacity);
}
```

Konštruktor vytvorí novú hash mapu.

### Čo robí:

- nastaví počet bucketov
- nastaví počet prvkov na 0
- pripraví `buckets` na požadovanú veľkosť

### `int capacity = 10`

To znamená, že ak pri vytváraní objektu nezadáme veľkosť, automaticky bude 10.

Príklady:

```c++
HashMap a;
```

→ použije kapacitu 10

```c++
HashMap b(5);
```

→ použije kapacitu 5

### `buckets.resize(capacity);`

Tým sa vytvorí daný počet prázdnych bucketov.

---

## 5. Metóda `put` — vloženie alebo aktualizácia

```c++
void put(string key, string value) {
    int h = hash(key);
    for (MapEntry& entry: buckets[h]) {
        if (entry.key == key) {
            entry.value = value;
            return;
        }
    }
    buckets[h].push_back(MapEntry(key, value));
    elementCount++;
}
```

Táto metóda:

- vloží nový pár `key -> value`
- alebo aktualizuje hodnotu, ak kľúč už existuje

---

### Podrobný postup

#### 1. Vypočíta hash index

```c++
int h = hash(key);
```

Nájde, do ktorého bucketu patrí daný kľúč.

---

#### 2. Prejde všetky prvky v danom buckete

```c++
for (MapEntry& entry: buckets[h]) {
```

Keďže v buckete môže byť viac položiek kvôli kolíziám, treba ich prejsť.

Tu je dôležité `MapEntry& entry`:

- ide o **referenciu**
- teda pracujeme priamo s pôvodným objektom v buckete
- vďaka tomu vieme meniť jeho `value`

---

#### 3. Ak kľúč už existuje, zmení hodnotu

```c++
if (entry.key == key) {
    entry.value = value;
    return;
}
```

Ak sa našiel rovnaký kľúč:

- neukladá nový prvok
- len prepíše hodnotu
- ukončí funkciu

To je správne správanie mapy: jeden kľúč má len jednu aktuálnu hodnotu.

---

#### 4. Ak sa kľúč nenašiel, pridá nový prvok

```c++
buckets[h].push_back(MapEntry(key, value));
elementCount++;
```

- vytvorí sa nový `MapEntry`
- vloží sa na koniec bucketu
- počet prvkov sa zvýši

---

## 6. Metóda `remove` — odstránenie prvku

```c++
void remove(string key) {
    int h = hash(key);
    for (int i = 0; i < buckets[h].size(); i++) {
        if (buckets[h][i].key == key) {
            buckets[h][i] = buckets[h][buckets[h].size() - 1];
            buckets[h].pop_back();
            elementCount--;
            return;
        }
    }
}
```

Táto metóda odstráni prvok s daným kľúčom, ak existuje.

---

### Podrobný postup

#### 1. Nájde správny bucket

```c++
int h = hash(key);
```

---

#### 2. Prechádza prvky podľa indexu

```c++
for (int i = 0; i < buckets[h].size(); i++) {
```

Tu sa ide cez indexy, nie cez `for-each`, pretože budeme odstraňovať prvok.

---

#### 3. Hľadá zhodu kľúča

```c++
if (buckets[h][i].key == key) {
```

---

#### 4. Samotné odstránenie

```c++
buckets[h][i] = buckets[h][buckets[h].size() - 1];
buckets[h].pop_back();
elementCount--;
return;
```

Toto je zaujímavý trik:

- prvok, ktorý chceme odstrániť, sa nahradí posledným prvkom bucketu
- potom sa posledný prvok zmaže pomocou `pop_back()`

### Prečo sa to robí?

Je to efektívnejšie než posúvať všetky ďalšie prvky doľava.

### Nevýhoda:

Zmení sa poradie prvkov v buckete.

V tomto programe to nevadí, pretože hash mapa nepotrebuje zachovávať poradie.

---

## 7. Metóda `containsKey`

```c++
bool containsKey(string key) {
    int h = hash(key);
    for (MapEntry entry: buckets[h]) {
        if (entry.key == key) return true;
    }
    return false;
}
```

Táto metóda zisťuje, či sa daný kľúč v mape nachádza.

### Postup:

- vypočíta bucket
- prejde všetky položky v buckete
- ak nájde zhodu, vráti `true`
- inak po skončení cyklu vráti `false`

### Poznámka

Tu sa používa:

```c++
for (MapEntry entry: buckets[h])
```

teda kópia prvku, nie referencia.  
Keďže sa objekt nemení, funguje to správne. Efektívnejšie by však bolo použiť referenciu, napríklad:

```c++
for (const MapEntry& entry : buckets[h])
```

Ale na pochopenie princípu je aktuálna verzia úplne v poriadku.

---

## 8. Metóda `get`

```c++
string get(string key) {
    int h = hash(key);
    for (MapEntry entry: buckets[h]) {
        if (entry.key == key) return entry.value;
    }
    throw runtime_error("No such key");
}
```

Táto metóda vráti hodnotu patriacu ku kľúču.

### Ako funguje:

- nájde správny bucket
- prejde položky v buckete
- ak nájde kľúč, vráti jeho hodnotu
- ak nie, vyhodí chybu

### `throw runtime_error("No such key");`

To znamená, že ak kľúč neexistuje, program oznámi chybu.

Je to podobné ako povedať:
> „Požadovaný kľúč sa v mape nenachádza.“

### Dôležité

Pri použití tejto metódy by bolo dobré vedieť, že môže skončiť výnimkou. Preto sa v praxi často pred `get()` volá
`containsKey()`.

---

## 9. Metóda `size`

```c++
int size() {
    return elementCount;
}
```

Veľmi jednoduchá metóda.

Vráti počet uložených prvkov v hash mape.

Ak máme uložené 4 dvojice kľúč-hodnota, vráti `4`.

---

## 10. Metóda `to_string`

```c++
string to_string() {
    string r = "";
    for (int i = 0; i < buckets.size(); i++) {
        r += std::to_string(i) + " : ";
        for (MapEntry entry: buckets[i]) {
            r += "[" + entry.key + "]" + entry.value + "; ";
        }
        r += "\n";
    }
    return r;
}
```

Táto metóda vytvorí textový výpis obsahu hash mapy.

---

### Čo robí:

- vytvorí prázdny reťazec `r`
- prejde všetky buckety
- ku každému vypíše jeho index
- potom vypíše všetky položky, ktoré sa v ňom nachádzajú

### Formát výpisu

Napríklad:

```c++
0 : [ab]baaaz; [ba]baz;
1 : [a]foo;
2 : [b]bar;
3 :
4 :
```

To je veľmi užitočné pri výučbe, lebo študent vidí:

- do ktorého bucketu sa kľúče dostali
- kde vznikli kolízie
- ako sa mení štruktúra po vložení či odstránení

---

## 11. Funkcia `main`

```c++
int main() {
    HashMap hm(5);
    hm.put("a", "foo");
    hm.put("b", "bar");
    hm.put("ab", "baaaz");
    hm.put("ba", "baz");

    cout << hm.to_string() << endl;

    hm.put("ba", "nova hodnota");

    cout << hm.to_string() << endl;

    hm.remove("a");

    cout << hm.to_string() << endl;

    return 0;
}
```

Tu sa ukazuje praktické použitie triedy `HashMap`.

---

### 11.1 Vytvorenie hash mapy

```c++
HashMap hm(5);
```

Vytvorí sa hash mapa s kapacitou 5 bucketov.

Teda indexy bucketov budú `0` až `4`.

---

### 11.2 Vkladanie prvkov

```c++
hm.put("a", "foo");
hm.put("b", "bar");
hm.put("ab", "baaaz");
hm.put("ba", "baz");
```

Do mapy sa vložia 4 položky.

### Zaujímavé je:

- `"ab"` a `"ba"` majú rovnaký súčet znakov
- preto pravdepodobne skončia v tom istom buckete

To je presne ukážka kolízie.

---

### 11.3 Prvý výpis

```c++
cout << hm.to_string() << endl;
```

Vypíše sa aktuálny stav hash mapy po vložení prvkov.

Študent tu vidí:

- ktoré buckety sú prázdne
- kde sa nachádzajú jednotlivé položky
- ktoré kľúče skončili spolu v jednom buckete

---

### 11.4 Aktualizácia existujúceho kľúča

```c++
hm.put("ba", "nova hodnota");
```

Tu sa nevkladá nový prvok.  
Keďže kľúč `"ba"` už v mape existuje, iba sa zmení jeho hodnota na `"nova hodnota"`.

To demonštruje, že `put()` neslúži len na vkladanie, ale aj na aktualizáciu.

---

### 11.5 Druhý výpis

```c++
cout << hm.to_string() << endl;
```

Teraz by mal byť obsah rovnaký ako predtým, ale hodnota pri kľúči `"ba"` už bude nová.

---

### 11.6 Odstránenie prvku

```c++
hm.remove("a");
```

Z mapy sa odstráni položka s kľúčom `"a"`.

---

### 11.7 Tretí výpis

```c++
cout << hm.to_string() << endl;
```

Nakoniec sa zobrazí stav mapy po odstránení prvku.

Takto je pekne vidieť celý životný cyklus údajov:

- vloženie
- aktualizácia
- odstránenie

---

## 12. Zhrnutie

Tento program ukazuje, ako možno od základov vytvoriť jednoduchú hash mapu:

- `MapEntry` predstavuje jednu dvojicu kľúč-hodnota
- `HashMap` obsahuje pole bucketov
- hash funkcia určuje, do ktorého bucketu prvok patrí
- kolízie sa riešia ukladaním viacerých prvkov do jedného bucketu
- metódy `put`, `remove`, `containsKey`, `get`, `size`, `to_string` ukazujú základné operácie nad mapou
- `main()` všetko demonštruje na konkrétnych príkladoch