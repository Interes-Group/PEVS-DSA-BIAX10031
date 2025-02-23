---
date: '2025-02-19T18:27:36+01:00'

title: 'Základy tried'
---

```cpp
#include <iostream>

using namespace std;

class Student {
public:
    string name;
    int year;

    /**
     * Enroll student for a provided year
     * @param new_year new year to enroll
     */
    void enroll(int new_year) {
        year = new_year;
    }
};

int main() {

    Student milan;
    const int item_size = 5;

    cout << "Zadaj meno študenta: ";
    cin >> milan.name;
    cout << "Ktorý ročník navštevuje? ";
    int year;
    cin >> year;
    milan.enroll(year);

    cout << "Študent " << milan.name << " navštevuje " << milan.year << ". ročník" << endl;
    
}
```

Tento program demonštruje základné princípy objektovo orientovaného programovania v C++, ako sú definícia triedy,
vytváranie objektov a interakcia s používateľom prostredníctvom konzolového vstupu a výstupu.

Program obsahuje definíciu triedy `Student` s atribútmi `name` a `year`, metódu `enroll` na nastavenie ročníka a funkciu
`main`, ktorá interaguje s
používateľom prostredníctvom konzoly. Nižšie je podrobný rozbor kódu s vysvetlením jednotlivých konceptov a odkazmi na
ďalšie zdroje:

```cpp
#include <iostream>
```

**Inklúzia knižnice**: Tento riadok zahrňuje štandardnú knižnicu `iostream`, ktorá umožňuje vstup a výstup dát (napr.
`cin` a `cout`).

```cpp
using namespace std;
```

**Deklarácia menného priestoru**: Umožňuje používať členy z menného priestoru `std` bez ich plnej kvalifikácie.
Napríklad namiesto `std::cout` stačí písať `cout`.

```cpp
class Student {
public:
    string name;
    int year;

    /**
     * Enroll student for a provided year
     * @param new_year new year to enroll
     */
    void enroll(int new_year) {
        year = new_year;
    }
};
```

**Definícia triedy**: Trieda `Student` obsahuje verejné (public) atribúty `name` a `year` a metódu `enroll`, ktorá
nastavuje hodnotu `year`. Viac o triedach a objektovo orientovanom programovaní
nájdete [tu](https://www.itnetwork.sk/cplusplus/programovanie-v-jazyku-cplusplus/cplusplus-kurz-oop/uvod-do-objektovo-orientovaneho-programovania-v-cplusplus).

```cpp
int main() {
    Student milan;
    const int item_size = 5;

    cout << "Zadaj meno študenta: ";
    cin >> milan.name;
    cout << "Ktorý ročník navštevuje? ";
    int year;
    cin >> year;
    milan.enroll(year);

    cout << "Študent " << milan.name << " navštevuje " << milan.year << ". ročník" << endl;
}
```

**Funkcia `main`**: Hlavná funkcia programu, kde sa vytvára inštancia triedy `Student` s názvom `milan`. Program
interaguje s používateľom prostredníctvom konzoly, načítava meno a ročník študenta a následne vypisuje tieto informácie.


