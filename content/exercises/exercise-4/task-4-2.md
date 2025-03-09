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

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
