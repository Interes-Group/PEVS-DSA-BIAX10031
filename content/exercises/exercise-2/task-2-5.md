---
date: '2025-02-14T18:11:31+01:00'
title: 'Úloha 2.5'
weight: 5
---

Napíšte program, zdrojový kód, v jazyku C++ použitím štandardu C++17, ktorý realizuje nasledovnú činnosť.

Navrhnite systém pre správu kurzov na univerzite. Systém by mal obsahovať triedy Kurz, Student a Univerzita. Trieda
Student by mala mať možnosť zapisovať sa na kurzy a trieda Univerzita by mala spravovať zoznam študentov a kurzov.

**Trieda `Kurz`:**

- _Atribúty_:
    - `std::string nazov`
    - `std::string kod`
    - `int kapacita`
    - `std::vector<Student*> zapisani_studenti`
- _Metódy_:
    - Konstruktor na inicializáciu názvu, kódu a kapacity kurzu.
    - Metóda na pridanie študenta do kurzu, ktorá skontroluje, či nie je prekročená kapacita.
    - Metóda na zobrazenie zoznamu zapísaných študentov.

**Trieda `Student`:**

- _Atribúty_:
    - `std::string meno`
    - `std::string id`
    - `std::vector<Kurz*> zapisane_kurzy`
- _Metódy_:
    - Konstruktor na inicializáciu mena a ID študenta.
    - Metóda na zápis do kurzu, ktorá pridá kurz do zoznamu zapísaných kurzov študenta.
    - Metóda na zobrazenie zoznamu kurzov, na ktoré je študent zapísaný.

**Trieda `Univerzita`:**

- _Atribúty_:
    - `std::vector<Student> studenti`
    - `std::vector<Kurz> kurzy`
- _Metódy_:
    - Metóda na pridanie nového študenta.
    - Metóda na pridanie nového kurzu.
    - Metóda na zápis študenta do kurzu na základe ID študenta a kódu kurzu.
    - Metóda na zobrazenie všetkých študentov a kurzov na univerzite.

Využite ukazovatele (`Student*` a `Kurz*`) na prepojenie študentov a kurzov, aby sa predišlo duplikácii údajov.
Pri pridávaní študenta do kurzu skontrolujte, či kapacita kurzu nie je prekročená, a ak áno, informujte o tom
používateľa. Dbajte na správne zapuzdrenie údajov a poskytnite potrebné metódy na prístup k súkromným atribútom. Pre
demonštráciu implementácie vytvorte vo funkcii `main` aspoň tri objekty a zavolajte ich metódy.

---

{{< details title="Rozbaľ pre ukážku riešenia" closed="true" >}}

Musím si počkať kým sa tu objaví príklad riešenia.

Nezabudni, že najviac sa naučíš ak to vypracuješ sám. 😉

{{< /details >}}
