---
date: '2025-02-14T18:11:24+01:00'

title: 'Úloha 2.2'
weight: 2
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Vytvorte triedu `BankovyUcet` so súkromnými atribútmi:

- `std::string cislo_uctu`
- `double zostatok`

**Implementujte verejné metódy:**

- Konstruktor na inicializáciu _čísla účtu_ a _počiatočného zostatku_.
- Metódu `vloz(double suma)`, ktorá zvýši zostatok o zadanú sumu.
- Metódu `vyber(double suma)`, ktorá zníži zostatok o zadanú sumu, ak je dostatok prostriedkov; inak vypíše varovanie o
  nedostatočnom zostatku.
- Metódu `ziskaj_zostatok()`, ktorá vráti aktuálny zostatok.

Zabezpečte, aby zostatok nemohol byť priamo modifikovaný mimo triedy. Ošetrite situácie, keď sa pokúsite vybrať viac
prostriedkov, než je dostupné na účte. Pre demonštráciu implementácie vytvorte vo funkcii `main` aspoň tri objekty a
zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
