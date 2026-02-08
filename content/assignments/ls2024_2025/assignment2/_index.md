---
date: '2025-04-14T00:23:36+02:00'
title: 'Zadanie 2'
---

# Prihlásenie sa na Semestrálne Zadanie 2

> [!IMPORTANT]
> Pre prihlásenie sa na zadanie 2 je potrebné mať aktívny GitHub účet. Účet je bezplatný.
> Pre vypracovanie môžte použiť existujúci GitHub účet.

Zadanie je distribuované aj odovzdané pomocou platformy GitHub, modulu Classroom. Pre prihlásenie sa na vypracovanie
zadania sa prihláste svojim účtom na platformu GitHub a následne kliknite na tento odkaz:

[https://classroom.github.com/a/S2BbF3Ve](https://classroom.github.com/a/S2BbF3Ve)

Po potvrdení sa vám vytvorí súkromný repozitár s nastaveným projektom a so znením zadania v súbore README.md (znenie
zadanie sa nachádza aj nižšie ⬇). **Pozorne si zadanie prečítajte pre začatím vypracovania.**

Vypracujte zadanie samostatne a podľa svojich najlepších vedomostí.
Implementáciu môžte priebežne _pushovať_ do repozitára až do uplynutia termínu, ktorý je uvedený v zadaní a bude
oznámený aj v MS Teams.

Pri každej aktualizácii kódu je spustená GitHub automatizácia, ktorý overí, či váš kód je skompilovateľný a spustiteľný.

V prípade otázok, problémov alebo akejkoľvek potreby konzultácií môžte kontaktovať cvičiaceho na MS Teams alebo emailom.

## Video s inštrukciami

Video je síce pôvodne pre zadaní 1, ale princíp je rovnaký aj pri tomto zadaní 2.

{{< youtube "mKHEAcSztfw" >}}

---

# Semestrálne zadanie 2 - Správa zoznamu úloh - TODO List

Cieľom zadania je implementovať jednoduchú aplikáciu manažéra úloh, tzv. TODO list pomocou jazyka C++ (štandard C++17).

Program má pracovať so zoznamom úloh, ktorý pomocou zadaných príkazov bude upravovať.
Zoznam úloh je ukladaný na disk do súboru. Program ponúka niekoľko príkazov a možností pre manipuláciu s úlohami.
Program neponúka používateľské menu, spracuje zadaný príkaz a skončí.

### Úloha (Task)

Úloha je objekt, ktorý má nasledovné atribúty:

- **názov** - Názov úlohy. Typ `string`. Názov úlohy je unikátny v rámci celého programu. Viaceré funkcionality
  vyhľadajú úlohu podľa názvu.
- **popis** - Dlhší popis o obsahu úlohy. Typ `string`.
- **priorita** - Priorita označujúca dôležitosť úlohy. Typ `int`. Nadobúda hodnoty kladných čísel (t.j. N > 0). Čím
  nižšie
  číslo tím je úloha dôležitejšia (priorita 1 == najdôležitejšia úloha).
- **termín dokončenia** - Dátum kedy má byť úloha dokončená. Typ `string`, formát ISO 8601 _yyyy-mm-dd_ (napr.
  2025-04-13).
- **status dokončenia** - Označenie či je úloha dokončená alebo nie. Typ `bool`. Hodnota true znamená, že úloha je
  dokončená.

## Funkcionalita

Aplikácia má poskytovať niekoľko príkazov, ktoré sú zadané ako argument spustenia programu spolu s ďalšími pomenovanými
parametrami (po anglicky options). Program poskytuje nasledovné funkcionality:

- pridanie novej úlohy
- vymazanie úlohy
- označenie úlohy za dokončenú
- vypísanie úloh pre určitý dátum
- vypísanie dostupnej funkcionality (tzv. help text)

### Pridanie úlohy

`zadanie2 add <-t --title názov_úlohy> [--desc popis_úlohy] [-p --priority priorita_úlohy] [-d --deadline termín_úlohy]`

Ak je ako prvý argument programu uvedený príkaz `add` je vytvorená nová úloha podľa zadaných parametrov:

- `-t` alebo `--title` _arg_ : Definovanie názvu novej úlohy. Argument za parametrom je hodnota názvu úlohy. Tento
  **parameter je povinný**.
- `--desc` _arg_ : Definovanie popisu novej úlohy. Argument za parametrom je hodnota popisu úlohy. Tento **parameter
  je voliteľný** a nemá defaultnú hodnotu.
- `-p` alebo `--priority` _arg_ : Definovanie priority úlohy. Argument za parametrom je celočíselná hodnota priority
  úlohy. Tento **parameter je voliteľný** a defaultná hodnota je 5.
- `-d` alebo `--deadline` _arg_ : Definovanie termínu splnenia úlohy. Argument za parametrom je reťazec dátumu vo
  formáte ISO 8601 t.j. _yyyy-mm-dd_ (ROK-MESIAC-DEŇ = 2025-04-14). Tento **parameter je voliteľný** a defaultná hodnota
  je dnešný dátum.

Ak nie je zadaný názov úlohy, program nič neurobí.

### Vymazanie úlohy

`zadanie2 remove <-t --title názov_úlohy>`

Ak je ako prvý argument uvedený príkaz `remove` podľa hodnoty v parametri `--title` je vymazaná úloha, ktorá je
definovaná uvedeným názvom. Príkaz pracuje s parametrom:

- `-t` alebo `--title` _arg_ : Definovanie názvu úlohy na vymazanie. Argument za parametrom je hodnota názvu úlohy.
  Tento **parameter je povinný**.

Ak názov úlohy nie je zadaný alebo úloha so zadaným názvom neexistuje, program vypíše adekvátnu hlášku a nič neurobí.
Ak je úloha so zadaným názvom nájdená, je vymazaná zo všetkých štruktúr programu.

### Označenie úlohy za dokončenú

`zadanie2 done <-t --title názov_úlohy>`

Ak je ako prvý argument uvedený príkaz `done` podľa hodnoty v parametri `--title` je úloha, ktorá je definovaná
uvedeným názvom, označená ako dokončená (atribút statusu dokončenia nastavený na true). Príkaz pracuje s parametrom:

- `-t` alebo `--title` _arg_ : Definovanie názvu úlohy na dokončenie. Argument za parametrom je hodnota názvu úlohy.
  Tento **parameter je povinný**.

Ak názov úlohy nie je zadaný alebo úloha so zadaným názvom neexistuje, program vypíše adekvátnu hlášku a nič neurobí.
Ak je úloha so zadaným názvom nájdená, úloha je označená ako dokončená.

### Vypísanie úloh pre určitý dátum

`zadanie2 list [-d --deadline termín_úlohy] [--done]`

`zadanie2 list [--today] [--done]`

`zadanie2 list [--tomorrow] [--done]`

Ak je ako prvý argument uvedený príkaz `list` sú vypísané úlohy podľa zadaných kritérií na základe uvedených parametrov:

- `-d` alebo `--deadline` _arg_ : Definovanie termínu úlohy. Argument za parametrom je reťazec dátumu vo
  formáte ISO 8601 t.j. _yyyy-mm-dd_ (ROK-MESIAC-DEŇ = 2025-04-14). Tento **parameter je voliteľný**.
- `--done` : Ak je tento parameter prítomný sú vypísané úlohy označené ako dokončené. Ak tento parameter nie je uvedený
  sú vypísané úlohy, ktoré ešte nie sú dokončené.
- `--today` : Ak je tento parameter uvedený, sú vypísané úlohy pre dnešný dátum. Tento parameter nie je možné kombinovať
  s parametrom `--deadline`. Ak sú uvedené oba, iba parameter `--deadline` je vyhodnotený.
- `--tomorrow` : Ak je tento parameter uvedený, sú vypísané úlohy pre nasledujúci deň. Tento parameter nie je možné
  kombinovať s parametrom `--deadline`. Ak sú uvedené oba, iba parameter `--deadline` je vyhodnotený.

V prípade ak nie sú zadané žiadne parametre sú vypísané všetky úlohy, ktoré má program načítané.

### Vypísanie dostupnej funkcionality

`zadanie2 [-h --help]`

Program vypíše tzv. help text, čo je zrozumiteľný výpis všetkých argumentov a parametrov programu a ich popis ako ich
použiť. Help text je aktivovaný cez zadaný parameter `--help` alebo skrátene `-h`.

Ak pri implementácií použijete knižnicu **cxxopts**, ktorá je zahrnutá v tomto projekte, tak vytvorený objekt
`cxxopts::Options` obsahuje metódu `string help()`, ktorá vráti naformátovaný string tzv. help textu, ktorý stačí
vypísať.

## Implementácia

V rámci implementácie programu môžte použiť všetky štandardné knižnice a funkcie jazyka C++, v štandarde C++17. Pre
implementáciu štruktúr môžte použiť knižnice STL (ako vector, queue, map a ďalšie).

### Knižnice

Je odporúčané použiť knižnice a funkcie, ktoré sú zahrnuté v tomto projekte. Pre ich použitie ich stačí zahrnúť do Vášho
zdrojového kódu: `#include "cxxopts.hpp"` a `#include "utils.hpp"` . Knižnice sú automaticky zahrnuté pri kompilácii
vďaka cmake konfigurácií. Zdrojové kódy knižníc sa nachádzajú v priečinku _include_.

Pre implementáciu spracovania argumentu a parametrov je odporúčané použiť knižnicu cxxopts
[https://github.com/jarro2783/cxxopts](https://github.com/jarro2783/cxxopts), ktorá je už zahrnutá v tomto projekte.

### Súbor tasks.db

Program má pri svojom spustení **načítať súbor `tasks.db`**. Ide o textový súbor, kde sú uložené všetky úlohy s ktorými
má program pracovať. Ak súbor pri spustený neexistuje je automaticky vytvorený v priečinku, kde je program spustený
(t.j. relatívna cesta _./tasks.db_). **Pred skončením programu** alebo po vykonaní zmeny úloh **sú úlohy ihneď uložené**
do súboru `tasks.db` pre perzistentné uloženie úloh pre ďalšie spustenie programu. Súbor má nasledovný formát pre
uloženie úloh:

```text
názov_úlohy|popis_úlohy|priorita|termín_úlohy|status_dokončenia_úlohy (0 = nedokončená, 1 = dokončená)
```

Jednotlivé atribúty **úlohy sú oddelené symbolom ' | '** (anglicky pipe). Status dokončenia úlohy je vyjadrený číslom 0
pre nedokončenú úlohu a číslom 1 pre dokončenú úlohu. Obsah súboru je pri každom uložení programom celý prepísaný
aktuálnym zoznamom úloh.

Príklad súboru:

```text
Spraviť zadanie|Treba spraviť vypracovanie na zadanie2|1|2025-04-28|0
Pozrieť si AVL strom|Mal by som sa bližšie pozrieť na AVL strom|2|2025-04-21|0
Prihlásiť sa na zadanie2||2|2025-04-14|1
```

### AVL strom

Úlohy implementujte ako C++ triedu. Pre zoznam úloh môžte použiť ľubovoľne zvolený STL kontajner či inú štruktúru,
ktorú uznáte za vhodnú pri zadanej funkcionalite. V rámci programu **implementujte vlastný AVL strom** (samo-vyvažovací
vyhľadávací strom), ktorý **má predstavovať index úloh podľa termínu úlohy**, t.j. podľa stringu vo formáte dátumu.
Pre porovnanie uzlov stromu použite funkciu [`int std:strcmp(const char *str1, const char *str2)`](https://www.geeksforgeeks.org/strcmp-in-c/).
V rámci uzla stromu má byť zoznam/vektor na referencie/pointre úloh, ktoré pripadajú pre daný dátum. Pridanie novej
úlohy má pridať aj pointer na túto úloh do stromu pre daný dátum a tak isto pri vymazaní úlohy má odstrániť pointer na
úlohu zo stromu. Viac o AVL strome sa môžte dozvedieť tu:

- [https://www.programiz.com/dsa/avl-tree](https://www.programiz.com/dsa/avl-tree)
- [https://www.geeksforgeeks.org/introduction-to-avl-tree/](https://www.geeksforgeeks.org/introduction-to-avl-tree/)
- [https://dsa.interes.group/examples/trees/avl_tree/](https://dsa.interes.group/examples/trees/avl_tree/)

### Zdrojové súbory

Implementácia môže byť umiestnená v jednom alebo viacerých zdrojových _.cpp_ súboroch. Program musí byť spúšťaný z main
súboru, `src/main.cpp`. V prípade rozdelenie kódu do viacerých súborov, nezabudnite pridať tieto zdrojové súbory do
príkazu `add_executable` v rámci súboru _src/CMakeLists.txt_ aby boli skompilované s celým programom.

### Git

Pre kontrolu kompilácie je v repozitáry nastavená automatizácia cez GitHub pipeline. Pipeline sa spustí automaticky pre
každý `git push` do vetvy `main`. GitHub pipeline sa pokúsi kód v repozitáry z posledného commitu skompilovať a následne
spustiť. Kontrola spustenia programu je voči výpisu tzv. help textu, t.j. je spustený príkaz `bin/zadanie2 --help`.
Pipeline je možné spustiť ľubovolný počet krát. Spustenie pipeline je možné vidieť v záložke "Actions" vo vašom
repozitáry vypracovania zadania.

## Hodnotenie

Zadanie je **ohodnotené 20 bodmi**. Odovzdanný program musí byť skompilovateľný (kompilátorom GCC, resp. G++) a
spustiteľný, inak je hodnotený 0 bodmi. Pri vypracovaní zadania sa kontroluje originalita vypracovaní a všetky
vypracovania **so zhodou vyššou ako 70% sú hodnotené 0 bodmi**. Hodnotený bude iba posledný commit vo vašom repozitáry
zadania, ktorý bol urobený do termínu odovzdania. Vypracovanie bude **podrobené automatizovaným testom**, ktoré budú
prevolávať program s definovanými parametrami a kontrolovať obsah výstupného súboru programu `tasks.db`. Je teda **veľmi
dôležité dodržať špecifikáciu vstupov a parametrov** v sekcií _Funkcionalita_ a **dodržať formát súboru `tasks.db`**.
Vypracovanie bude hodnotené aj ručne. Pri hodnotení vypracovania sa bude prihliadať na:

- práca so štrutkúrami
- využitie STL knižníc
- implementácia AVL stromu
- práca s pamäťou (alokácia a uvoľnenie pamäte)
- využitie tried (použitie princípov enkapsulácie)
- využitie podmienok/control flow (if, if-else a pod.)
- korektnosť kódu
- prezentácia údajov používateľovi

## Odovzdanie

Po prihlásení sa na zadanie 2 cez [GitHub Classroom](https://classroom.github.com/) vám je automaticky vytvorená kópia
repozitára, ktorý bude nastavený ako privátny pre vás na vypracovanie (t.j. máte povolený commit a push do repozitára).
Skontrolujte si, či sa váš repozitár nachádza pod skupinou _Interes-Group_, inak vyučujúci nemajú prístup ku vášmu
repozitáru a zadanie sa nepovažuje za odovzdané. Ak sa vám repozitár nevytvorí ihneď kontaktuje vyučujúceho na MS Teams
alebo na univerzitnom emaily. Je prísne zakázané dávať prístup k vášmu repozitáru inému študentovi, alebo osobe, ktorá
nie je vyučujúci. V repozitáry by sa mali vytvoriť dve vetvy _main_ a _feedback_ a vytvorí sa tzv. pull request z vetvy
_main_ do vetvy _feedback_. Váš **kód pushujte do vetvy main**. Hodnotenie vypracovania, ako aj komentár ku kódu,
uvidíte v danom pull requeste. Ak sa vám vetva _feedback_ alebo pull request nevytvorí ihneď kontaktuje vyučujúceho na
MS Teams alebo na univerzitnom emaily.

V repozitáry upravujte iba súbory pod priečinkom `src`. Ostatné súbory je zakázané upravovať, predovšetkým súbory pre
kompiláciu, súbory obsahujúce github pipeline-y (`.github`) a súbory obsahujúce automatizované testy (`tests`). Pri
zmene týchto súborov môže byť vypracovanie ohodnotené 0 bodmi.

Vypracovanie zadania priebežne commitujte/pushujte do repozitára vytvoreného GitHubom pre toto zadanie. Váš kód
commitujte/pushujte do vetvy `main`. Hodnotený bude posledný commit/push do termínu odovzdania vypracovania.
Vypracovanie je **nutné odovzdať**/commitnúť/pushnúť do repozitára **do 28.04.2025 23:59**. Neodovzdanie je hodnotené 0
bodmi.

V prípade otázok, alebo technických problémov môžete kontaktovať vyučujúcich na MS Teams alebo na univerzitnom emaily.
