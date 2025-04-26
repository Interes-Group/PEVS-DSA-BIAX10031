---
date: '2025-04-26T19:07:43+02:00'
title: 'C++ Templates'
---

V C++ sú **šablóny (templates)** mechanizmus generického programovania, ktorý umožňuje písať funkcie alebo triedy
parametrizované typom (alebo aj inými hodnotami). Kompilátor pri použití šablóny automaticky „vygeneruje“ konkrétnu
verziu pre každý skutočný typ, ktorý sa pri vyvolaní použije.

---

## 1. Základná definícia funkčnej šablóny

```cpp
template<typename Tp>  // alebo template<class Tp>
void print_vector(vector<Tp> &v) {
    for (auto i : v)
        cout << i << " ";
    cout << endl;
}
```

- `template<typename Tp>` hovorí kompilátoru:
  > „Nasledujúca funkcia bude parametrizovaná jedným typovým parametrom `Tp`.”

- V tele funkcie potom namiesto konkrétneho `int`, `double` či nejakého iného typu používame `Tp`.

---

## 2. Ako kompilátor generuje kód (instantiácia)

Keď v `main()` napíšete:

```cpp
vector<int>  vi = {1,2,3};
vector<string> vs = {"a","b","c"};

print_vector(vi);   // Tp := int
print_vector(vs);   // Tp := std::string
```

- Kompilátor v momente prvého volania `print_vector(vi)` vytvorí **verziu** `print_vector<int>(vector<int>&)`.
- Pri `print_vector(vs)` zasa vznikne `print_vector<std::string>(vector<std::string>&)`.
- Vznikajú teda dve samostatné funkcie – ani jedna nevyžaduje runtime „kontrolu typu”, všetko sa rieši počas prekladu.

---

## 3. Výhody funkčných šablón

1. **Žiadne duplicitné písanie kódu**  
   Nemusíte robiť overloady pre `vector<int>`, `vector<double>`, `vector<char>` atď. – jedna šablóna pokryje všetky.
2. **Typová bezpečnosť**  
   Ak sa pokúsite zavolať `print_vector(42)`, kompilátor vyhodí chybu, pretože neexistuje `vector<Tp>` pre jeden `int`.
3. **Výkon**  
   Vygenerovaný kód je špecializovaný a optimalizovaný pre konkrétny typ, bez dynamic dispatch či virtuálnych volaní.

---

## 4. Použitie viacerých parametrov

Okrem jedného typu môžete mať niekoľko:

```cpp
template<typename Key, typename Value>
pair<Key, Value> make_pair(const Key& k, const Value& v) {
    return {k, v};
}
```

- `make_pair<int, string>(42, "abc")` vygeneruje pár `(int, string)`.

---

## 5. Non-type šablónové parametre

Šablóna môže mať aj _hodnotový_ parameter:

```cpp
template<typename T, size_t N>
void print_array(const T (&arr)[N]) {
    for (size_t i = 0; i < N; i++)
        cout << arr[i] << " ";
    cout << endl;
}
```

- Tu `N` je konštanta (napr. veľkosť poľa) známa pri kompilácii.

---

## 6. Špecializácie

Ak potrebujete pre konkrétny typ zmeniť správanie, definujete špecializáciu:

```cpp
template<>
void print_vector<bool>(vector<bool> &v) {
    // špeciálna implementácia pre bool (ktoré v STL absence priamych bool&)
}
```

- Prázdny `<…>` za `template` a typ vo zátvorke hovoria, že ide o špeciálny prípad.

---

### Zhrnutie

- **Šablóna** sa definuje kľúčovým slovom `template<…>`.
- **Typový parameter** (`typename` alebo `class`) umožňuje písať generický kód.
- Kompilátor **pri použití** (instantiácii) vytvorí konkrétne verzie funkcie/triedy.
- Použitie šablón zvyšuje **znovupoužiteľnosť** a **typovú bezpečnosť** bez straty výkonu.
