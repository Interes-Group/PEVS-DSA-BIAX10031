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

```cpp
#include <iostream>
#include <vector>
#include <string>

// Forward deklarácia triedy Student pre správne fungovanie ukazovateľov v triede Kurz
class Student;

class Kurz {
private:
    std::string nazov;
    std::string kod;
    int kapacita;
    std::vector<Student*> zapisani_studenti;

public:
    // Konštruktor na inicializáciu kurzu
    Kurz(const std::string& nazov, const std::string& kod, int kapacita)
        : nazov(nazov), kod(kod), kapacita(kapacita) {}

    std::string getKod() const { return kod; }
    std::string getNazov() const { return nazov; }

    // Metóda na pridanie študenta do kurzu
    bool pridajStudenta(Student* student);

    // Výpis zapísaných študentov
    void zobrazZapisanychStudentov() const;
};

class Student {
private:
    std::string meno;
    std::string id;
    std::vector<Kurz*> zapisane_kurzy;

public:
    // Konštruktor na inicializáciu študenta
    Student(const std::string& meno, const std::string& id)
        : meno(meno), id(id) {}

    std::string getId() const { return id; }
    std::string getMeno() const { return meno; }

    // Metóda na zápis do kurzu
    void zapisDoKurzu(Kurz* kurz) {
        zapisane_kurzy.push_back(kurz);
    }

    // Výpis zapísaných kurzov
    void zobrazZapisaneKurzy() const {
        std::cout << "Študent: " << meno << " (" << id << ") je zapísaný na kurzy:\n";
        if (zapisane_kurzy.empty()) {
            std::cout << "  - Žiadne zapísané kurzy\n";
        } else {
            for (const auto& kurz : zapisane_kurzy) {
                std::cout << "  - " << kurz->getNazov() << " (" << kurz->getKod() << ")\n";
            }
        }
        std::cout << "----------------------\n";
    }
};

// Implementácia metódy na pridanie študenta do kurzu
bool Kurz::pridajStudenta(Student* student) {
    if (zapisani_studenti.size() < static_cast<size_t>(kapacita)) {
        zapisani_studenti.push_back(student);
        student->zapisDoKurzu(this);
        return true;
    } else {
        std::cout << "Kurz " << nazov << " je plný! Študent " << student->getMeno() << " sa nemôže zapísať.\n";
        return false;
    }
}

// Implementácia metódy na výpis zapísaných študentov v kurze
void Kurz::zobrazZapisanychStudentov() const {
    std::cout << "Kurz: " << nazov << " (" << kod << ") - Kapacita: " << kapacita << "\n";
    std::cout << "Zapísaní študenti:\n";
    if (zapisani_studenti.empty()) {
        std::cout << "  - Žiadni študenti nie sú zapísaní\n";
    } else {
        for (const auto& student : zapisani_studenti) {
            std::cout << "  - " << student->getMeno() << " (" << student->getId() << ")\n";
        }
    }
    std::cout << "----------------------\n";
}

class Univerzita {
private:
    std::vector<Student> studenti;
    std::vector<Kurz> kurzy;

public:
    // Metóda na pridanie nového študenta
    void pridajStudenta(const std::string& meno, const std::string& id) {
        studenti.emplace_back(meno, id);
    }

    // Metóda na pridanie nového kurzu
    void pridajKurz(const std::string& nazov, const std::string& kod, int kapacita) {
        kurzy.emplace_back(nazov, kod, kapacita);
    }

    // Metóda na zápis študenta do kurzu
    void zapisStudentaDoKurzu(const std::string& studentId, const std::string& kodKurzu) {
        Student* studentPtr = nullptr;
        Kurz* kurzPtr = nullptr;

        // Nájdeme študenta podľa ID
        for (auto& student : studenti) {
            if (student.getId() == studentId) {
                studentPtr = &student;
                break;
            }
        }

        // Nájdeme kurz podľa kódu
        for (auto& kurz : kurzy) {
            if (kurz.getKod() == kodKurzu) {
                kurzPtr = &kurz;
                break;
            }
        }

        // Skontrolujeme, či sa našli obe entity
        if (studentPtr && kurzPtr) {
            kurzPtr->pridajStudenta(studentPtr);
        } else {
            std::cout << "Chyba: Študent alebo kurz nebol nájdený.\n";
        }
    }

    // Metóda na zobrazenie všetkých študentov a kurzov
    void zobrazStudentovAKurzy() const {
        std::cout << "Zoznam študentov:\n";
        if (studenti.empty()) {
            std::cout << "  - Žiadni študenti nie sú registrovaní\n";
        } else {
            for (const auto& student : studenti) {
                student.zobrazZapisaneKurzy();
            }
        }

        std::cout << "Zoznam kurzov:\n";
        if (kurzy.empty()) {
            std::cout << "  - Žiadne kurzy nie sú registrované\n";
        } else {
            for (const auto& kurz : kurzy) {
                kurz.zobrazZapisanychStudentov();
            }
        }
    }
};

int main() {
    Univerzita univerzita;

    // Pridanie kurzov
    univerzita.pridajKurz("Matematika", "MAT101", 2);
    univerzita.pridajKurz("Programovanie", "PROG202", 3);
    univerzita.pridajKurz("Fyzika", "FZK303", 1);

    // Pridanie študentov
    univerzita.pridajStudenta("Ján Novák", "S001");
    univerzita.pridajStudenta("Mária Horváthová", "S002");
    univerzita.pridajStudenta("Peter Kováč", "S003");

    // Zápis študentov do kurzov
    univerzita.zapisStudentaDoKurzu("S001", "MAT101");
    univerzita.zapisStudentaDoKurzu("S002", "MAT101");
    univerzita.zapisStudentaDoKurzu("S003", "MAT101"); // Tento zápis by mal zlyhať (kapacita 2)

    univerzita.zapisStudentaDoKurzu("S002", "PROG202");
    univerzita.zapisStudentaDoKurzu("S003", "FZK303");
    univerzita.zapisStudentaDoKurzu("S001", "PROG202");

    // Zobrazenie študentov a kurzov
    univerzita.zobrazStudentovAKurzy();

    return 0;
}
```

### **Vysvetlenie:**
- **Trieda `Kurz`** umožňuje pridávať študentov do kurzu s kontrolou kapacity.
- **Trieda `Student`** ukladá meno, ID a zoznam zapísaných kurzov.
- **Trieda `Univerzita`** spravuje zoznam študentov a kurzov a umožňuje zapisovať študentov do kurzov na základe ID.

### **Príklad výstupu:**
```
Kurz Matematika je plný! Študent Peter Kováč sa nemôže zapísať.
Zoznam študentov:
Študent: Ján Novák (S001) je zapísaný na kurzy:
  - Matematika (MAT101)
  - Programovanie (PROG202)
...
```

{{< /details >}}
