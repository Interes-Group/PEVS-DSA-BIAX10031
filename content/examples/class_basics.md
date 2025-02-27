---
date: '2025-02-27T11:55:36+01:00'
title: 'Základy tried'
---

```cpp
#include <iostream>

using namespace std;

/**
 * Trieda reprezentujúca študenta
 */
class Student {
private:
    /**
     * Private člen/atribút triedy pre meno.
     * Chceme aby ak raz sa vyplní tak bol nemenný.
     */
    string name;

    /**
     * Private člen/atribút triedy pre vek.
     * Môže sa meniť v čase.
     */
    int age;
public:

    /**
     * defaultný konštruktor
     * Potrebný ak chceme vytvoriť objekt bez hodnôt v atribútoch.
     */
    Student() = default;

    /**
     * Konštruktor len s inicializáciou mena.
     * @param n meno študenta
     */
    Student(const string &n) : name{n} {}

    /**
     * Konštruktor pre nastavenie oboch atribútov triedy.
     * @param n meno študenta
     * @param a vek študenta
     */
    Student(const string &n, const int a)
            : name{n}, age{a} {}

    /**
     * Získanie mena študenta.
     * @return meno študenta
     */
    string get_name() {
        return name;
    }

    /**
     * Nastavenie mena študenta.
     * Meno študenta je nastavené jedine vtedy ak ešte v minulosti nastavené nebolo,
     * t.j. ak objekt bol inicializovaný bez mena.
     * Ak meno študenta už má hodnotu, vstup metódy je ignorovaný.
     * @param name nové meno študenta
     */
    void set_name(const string &name) {
        if (Student::name == "") { // každý string je defaultne inicializovaný na prázdny, t.j. ""
            Student::name = name;
        }
    }

    /**
     * Získanie veku študenta.
     * @return vek študenta
     */
    int get_age() const {
        return age;
    }

    /**
     * Nastavenie veku študenta
     * @param age nový vek študenta
     */
    void set_age(int age) {
        Student::age = age;
    }

    /**
     * Serializácia objektu študent do typu string.
     * Metóda vhodná pre potreby výpisu, či uloženie do súboru.
     * @return serialozovaný objekt študenta
     */
    string to_string() {
        return "Student{name:" + name + ", age:" + std::to_string(age) + "}";
    }

};


int main() {
    Student milan; // použitý defaultný konštruktor
    Student jano("Jano"); // použitý konštruktor iba s menom
    Student magda("Magda", 25); // použitý konštruktor s oboma atribútami

    milan.set_name("Milan");
    milan.set_age(33);
    cout << milan.to_string() << endl;

    jano.set_name("Martin"); // meno sa už nezmení nakoľko objekt jano už mená má
    cout << jano.to_string() << endl;

    return 0;
}
```

Tento program demonštruje základné princípy objektovo orientovaného programovania v C++, ako sú definícia triedy,
vytváranie objektov a interakcia s používateľom prostredníctvom konzolového vstupu a výstupu.

### Trieda `Student`

- **Atribúty:**
    - `name` (typ `string`): Meno študenta, ktoré môže byť nastavené len raz (nemenné po inicializácii).
    - `age` (typ `int`): Vek študenta, ktorý môže byť meniaci sa.

- **Konštruktory:**
    - **Defaultný konštruktor:**  
      Umožňuje vytvorenie objektu `Student` bez inicializácie atribútov.
    - **Konštruktor s menom:**  
      Inicializuje atribút `name`.
    - **Konštruktor s menom a vekom:**  
      Inicializuje oba atribúty `name` a `age`.

- **Metódy:**
    - `string get_name()`:  
      Vráti meno študenta.

    - `void set_name(const string &name)`:  
      Nastaví meno študenta iba v prípade, že zatiaľ nebolo nastavené (kontroluje prázdny reťazec). Zabezpečuje
      nemennosť mena po prvom nastavení.

    - `int get_age() const`:  
      Vráti aktuálny vek študenta.

    - `void set_age(int age)`:  
      Nastaví nový vek študenta.

    - `string to_string()`:  
      Serializuje údaje študenta do reťazca v tvare:  
      `"Student{name:<meno>, age:<vek>}"`  
      Táto metóda je vhodná na výpis alebo ukladanie údajov.

### Dôležité vlastnosti:

1. **Zapuzdrenie:**  
   Atribúty `name` a `age` sú súkromné, prístup k nim je riadený verejnými metódami.

2. **Kontrola nemennosti mena:**  
   Atribút `name` môže byť nastavený iba raz a následné pokusy o zmenu sa ignorujú.

3. **Serializácia:**  
   Metóda `to_string()` umožňuje jednoduchý výpis alebo uloženie údajov o študentovi.
