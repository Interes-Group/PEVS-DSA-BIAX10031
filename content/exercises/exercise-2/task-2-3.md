---
date: '2025-02-14T18:11:26+01:00'

title: 'Úloha 2.3'
weight: 3
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte triedu `Student` so súkromnými atribútmi:

- `std::string meno`
- `int rocnik`
- `std::vector<double> znamky`

**Implementujte verejné metódy:**

- Konstruktor na inicializáciu _mena_ a _ročníka_.
- Metódu `pridaj_znamku(double znamka)`, ktorá pridá známku do zoznamu známok.
- Metódu `vypocitaj_priemer()`, ktorá vráti priemernú hodnotu známok.
- Metódu `vypis_info()`, ktorá vypíše informácie o študentovi v nasledujúcom formáte:

```text
Meno: <meno>
Ročník: <rocnik>
Priemerná známka: <priemer>
```

Použite triedu `std::vector` na ukladanie známok. Zabezpečte, aby meno a ročník nemohli byť priamo modifikované mimo
triedy. Ošetrite situácie, keď študent nemá žiadne známky pri výpočte priemeru. Pre demonštráciu implementácie vytvorte
vo funkcii `main` aspoň tri objekty a zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
