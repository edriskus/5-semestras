# Programų sistemų projektavimas

## Įvadas

- Programinės įrangos **iššūkiai**:
    - Programinė įranga
        - yra naudojama ilgą laiką
        - yra nuolat atnaujinama ir prižiūrima
        - programuotojų kurie nėra autoriai
    - Pradinė specifikacija gali būti negalutinė
    - Galutinių vartotojų poreikiai ir lūkesčiai keičiasi
    - Keičiasi aplinka
- Sistemos tampa **per sudėtingos**, nes:
    - Nėra stiprios komandos kuri rūpinasi sistemos architektūra.
    - Programuotojų kompetencijos  ir patirties trūkumas
    - Trūksta profesionalaus projekto valdymo.
- **Dekompozicija** - *Skaldyk ir valdyk principas.*
- **Abstrakcija** - *Paslėpti sudėtingus dalykus. Programuotojai dirba su apibrėžta sąsaja.*
- **Hierarchija** - *Klasifikuoti objektus pagal panašią elgseną*
- **PS Architektūra:**
    - Sistemos griaučiai
    - Kokios duomenų saugyklos bus naudojamos
    - Kaip atskiri moduliai bendraus vieni su kitais
    - Kaip darysim atsargines duomenų kopijas
    - Ir t.t.
- **PS projektavimas:**
    - Atskirų modulių projektavimas
    - Kokios atsakomybės modulio, komponento, konkrečios klasės.
    - Kokius projektavimo šablonus pritaikyti
    - Ir t.t.
- **Bottom-up design:** Identifikuojami ir suprogramuojami mažesni sistemos moduliai, vėliau apjungiami į vientisą sprendimą.
- **Kokybės**, kurių siekiame projektuodami programų sistemas:
    - Paprastumas. *KISS (Keep It Simple, Stupid) principas. YAGNI (You Aren’t Gonna Need It) principas. Vengiama Over-engineering*
    - Palaikomumas (**Maintainability**). *Kaip greitai taisomos klaidos. Kaip greitai pridedamas naujas funkcionalumas*
    - Spartumas (**Performance**)
    - Suderinamumas (**Compatibility**) *su kitomis sistemomis*
    - Saugumas (**Security**)
    - Plečiamumas (**Scalability**)

---

## Kokybiškas programinis kodas. Refaktoringas

- **Clean code:**
    - Užtikrina visus funkcinius reikalavimus
    - Lengvai skaitomas ir suprantamas
    - Lengvai keičiamas
    - Užtikrina kitus nefunkcinius reikalavimus (efektyvumas, saugumas, stabilumas, …)
- Blogo kodo **priežastys:**
    - Laiko stoka
    - Reikalavimų nežinojimas
    - Reikalavimų kitimas
    - Disciplinos trūkumas
    - … NEPROFESIONALUMAS
- Blogo kodo **pasekmės:**
    - Blogo kodo daugėja
    - Ilgėja užduočių atlikimo terminai
    - Daugėja klaidų
    - Auga kaštai
    - Psichologinės pasekmės: auga stresas, krenta motyvacija

### Kaip programinio kodo kokybė siejasi su dizaino kokybe?

- Sunkiai **suprantamame** kode:
    - sunku įsitikinti ar laikomasi sistemos projekto
    - sunku įžvelgti kodo bendrumus ir bendras logines abstrakcijas
    - sunku įžvelgti dizaino problemas ir loginius prieštaravimus
- Sunkiai **keičiamame** kode:
    - sunku užtikrinti keliamas kokybes
    - sunku vystyti sistemą (ir jos projektą)
- **Kento Beko** paprasto dizaino taisyklės:
    - Runs all the tests
    - Contains no duplication
    - Expresses the intent of the programmer
    - Minimizes the number of classes and methods

### Švaraus programinio kodo taisyklės
- **Nuoseklus kodo formatavimas**
<br>*Visas kodas turi atrodyti taip, tarsi jį būtų parašęs tas pats žmogus.*

- **Prasmingi, tikslūs pavadinimai**
<br>*Pavadinimai turi atitikti probleminės srities žodyną. Funkcijos turi būti veiksmažodžiai (arba akcesoriai isXXX(), hasXXX(), containsXXX() ). Klasės ir kintamieji turi būti daiktavardžiai.*

- **Trumpos funkcijos ir klasės**
<br>*Viena funkcija turi atlikti vieną ir tik vieną dalyką, bet - išimtys (testai, GUI, ...)*

- **Viena, aiški paskirtis**
<br>*Viena funkcija turi atlikti tik vieną dalyką. Viena klasė turi turėti vieną atsakomybę. Praktikoje tai reiškia daug mažų klasių ir funkcijų.*

- **Jokių pašalinių efektų**
<br>**

- **Komentarų problematika**
<br>*Užkomentuotas kodas rodo nemokėjimą naudotis kodo versijonavimo sistemomis.*

- **OOP / OOD reikalavimų laikymasis**
<br>**

- **Nuoseklus susitarimų laikymasis**
<br>*“Visas programinis kodas turi atrodyti taip, tarsi jį būtų parašęs vienas žmogus”.*

- **“Skautų taisyklė”**
<br>*Always check a module in cleaner than when you checked it out.*

---

- **Refaktoringas** - Programinio kodo keitimo procesas, kurio metu gerinama kodo kokybė, nekeičiant funkcionalumo. Naudojant standartinius, nedidelius žingsnelius įmanoma iš esmės pagerinti kodą bei išgryninti PS projektinius sprendimus (bottom-up projektavimas).
- **Code smell:**
    - Ilgi metodai ar klasės
    - Sudėtingi metodai
    - Dublikuotas kodas
    - Komentarai
    - Magiški skaičiai ir tekstinės reikšmės
- Refaktorinimai:
    - **Extract Method** - dalis kodo iškeliama į atskirą procedūrą.
    - **Rename Method** - funkcijos pavadinimas pakeičiamas į suprantamesnį.
    - **Inline Method** - funkcija ištrinama, o jos kodas įterpiamas ten, iš kur ji buvo kviečiama (priešingas žingsnis Extract Method refaktoringui)
    - **Extract Class** - dalis kintamųjų ir funkcijų išskiriami į atskirą klasę.
    - **Inline Class** -
    - **Introduce Explaining Variable** - sudėtingo skaičiavimo dalis priskiriama suprantamai pavadintam kintamajam, kurio vieninintelė paskirtis tėra paaiškinti skaičiavimo logiką.
    - **Inline Temp**

---

## Objektiškai orientuoto projektavimo principai

### Pagrindinės OO koncepcijos

- **Abstrakcija** - atimam neesmines detales kad lengviau suprastume.
- **Inkapsuliacija** - užtikrina patikimus pakeitimus
- **Paveldėjimas** (extend, is-a) - Kodo perpanaudojamumas, plečiamumas
- **Polimorfizmas** (implement, has-a) - OOP mechanizmas, kai skirtingų tipų objektai palaiko tą patį interfeisą.

### Pagrindinės OO metrikos

- Priklausomybė (**Coupling**) - Klasės turi būti “loose coupled”, t.y., kuo mažiau kitų klasių turi reikėti “mūsų klasės” funkcionavimui
    - Law of demeter - nesileisti gilyn į objektų objektus
- Kohezija (**Cohesion**) - Klasės vidiniai elementai (kintamieji, metodai) turi būti stipriai susiję (jokio pašalinio funkcionalumo).
- Pakankamumas (**Sufficiency**) - Klasė turi pateikti pakankamai abstrakcijos charakteristikų kurios leidžia prasmingą ir efektyvų panaudojamumą. 
- Išbaigtumas (**Completeness**) - Jei klasė turi visą išbaigtą funkcionalumą ir sukurta perpanaudojamui - tokia klasė yra išbaigta.
- Primytivumas (**Primitiveness**) - palikti tik esmines operacijas. Operacija nėra primityvi (t.y., operacija yra perteklinė), jei ji sudaryta vien tik iš kitų public operacijų.

### Pagrindiniai OOD principai (SOLID)

- **Single responsibility principle** - klasė atlieka tik vieną darbą
- **Open-closed principle** - Objektai turi būti atviri praplėtimui, bet uždari modifikavimui.
- **Liskov substitution principle** - “Vaikinės klasės niekada neturi sugriauti tėvinių klasių numatytos elgsenos.”, Barbara Liskov, 1987m.
- **Interface segregation principle** - Vartotojas niekada neturi būti verčiamas implementuoti interfeisą kurio nenaudos ar priklausyti nuo metodų kurių nepanaudos.
- **Dependency inversion principle** - Esybės turi priklausyti ne nuo konkrečių implementacijų, bet nuo abtrakcijų.

---

## Projektavimo šablonai (Design Patterns)

- *Perpanaudojami sprendimai problemos projektavimui*
- **Strategy** projektavimo šablonas apibrėžia panašius atskirai inkapsuliuotus algoritmus ir padaro juos dinamiškai pakeičiamus. Tai pat leidžia algoritmams kisti nepriklausomai nuo klientų kurie juos naudoja.
- Šablonų struktūra:
    - Šablono pavadinimas
    - Problema
    - Sprendimas
    - Pasekmės
- **Creational** patterns - Rūpinasi objektų sukūrimu ir leidžia atsieti klientą nuo objektų, kuriuos jam reikia sukurti.
    - **Abstract Factory** - Creates an instance of several families of classes
    - **Builder** - Separates object construction from its representation
    - **Factory Method** - Creates an instance of several derived classes
    - **Object Pool** - Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
    - **Prototype** - A fully initialized instance to be copied or cloned
    - **Singleton**	- A class of which only a single instance can exist
- **Structural** patterns - Leidžia sukomponuoti klases ar objektus į sudėtingesnes struktūras
    - **Adapter** - Match interfaces of different classes
    - **Bridge** - Separates an object's interface from its implementation
    - **Composite** - A tree structure of simple and composite objects
    - **Decorator** - Add responsibilities to objects dynamically
    - **Facade** - A single class that represents an entire subsystem
    - **Flyweight** - A fine-grained instance used for efficient sharing
    - **Private Class Data** - Restricts accessor/mutator access
    - **Proxy** - An object representing another object
- **Behavioural** patterns - Kaip klasės ir objektai bendrauja tarpusavyje ir pasidalina atsakomybes.
    - **Chain of responsibility** - A way of passing a request between a chain of objects
    - **Command** - Encapsulate a command request as an object
    - **Interpreter** - A way to include language elements in a program
    - **Iterator** - Sequentially access the elements of a collection
    - **Mediator** - Defines simplified communication between classes
    - **Memento** - Capture and restore an object's internal state
    - **Null Object** - Designed to act as a default value of an object
    - **Observer** - A way of notifying change to a number of classes
    - **State** - Alter an object's behavior when its state changes
    - **Strategy** - Encapsulates an algorithm inside a class
    - **Template method** - Defer the exact steps of an algorithm to a subclass
    - **Visitor** - Defines a new operation to a class without change

<br>

- **Anti-Patterns:** Pasikartojanti situacija, kuri sukelia problemas užuot jas sprendus. Keli pavyzdžiai:
    - The God class
    - Golden Hammer
    - Spaghetti Code 

---

## Kodo testavimas (Unit Testing) ir jo įtaka projektavimui

### Unit testing

-*A unit test is an automated piece of code that invokes a unit of work in the system and then checks a single assumption about the behavior of that unit of work.*
- **Regression** - Funkcionalumas, kuris veikė prieš tai - nebeveikia. 
- Unit testai turi būti:
    - Greiti
    - Patikimi
    - Tikslūs
    - Lengvai skaitomi ir rašomi
- Struktūra (rekomenduojama):
    - Arrange - paruošimas
    - Act - vykdymas
    - Assert - tikrinimas
- **Dažnos ydos:**
    - Testai rašomi vėliau negu kodas.
    - Testus rašo ne kodo autorius.
    - Testų rašymas perleidžiamas testuotojams (“nes jie gi testuotojai”).
    - Testų kodui skiriamas nepakankamas dėmesys.
    - Testai priklauso vieni nuo kitų
    - Viename teste tikrinami keli skirtingi atvejai

### Projektavimo įtaka testuojamumui / Testavimo įtaka projektavimui

- *Esminė problema - blogai suprojektuotos klasės apsunkina testavimą.*
- Kaip to išvengti:
    - Naudoti DI (Dependency Injection)
        - **Dependency injection** - projektavimo šablonas kuris įgyvendina “dependency inversion” principą (SOLID).  
        - Objektas gauna visus reikalingus objektus (priklausomus objektus, “priklausomybes”) per konfigūraciją (konstruktoriuje)
    - Dirbti su interfeisais
    - Vengti sudėtingų statinių metodų

### Mocking

- *Objektai, simuliuojantys “tikrų objektų” elgesį.*
- *Priklausomybių eliminavimas/imitavimas*
- Privalumai:
    - Sparta
    - Patikimumas
    - Skaitomumas

### TDD

- *Test driven development*
1. Parašyti ir paleisti neveikiantį test’ą.
2. Implementuoti tiek, kad testai veiktų.
3. Refaktorinti kodą, gerinti kodo kokybę

- Teisingai taikant TDD praktikas unit testai tampa detaliomis specifikacijomis, kurios sukurtos kaip tik laiku.
- Programuotojai tipiškai yra linkę dirbti su kodu, o ne su dokumentacija.
- Bandant suprasti klasę ar metodą pirmiausiai programuotojai žiūrės į kodo pavyzdžius, kurie kviečia metodus.
- Testai sukurti kaip specifikacijos daro būtent tai!

---

## Išankstinis projektavimas (upfront design)

- **In a nutshell:**
    - Taikomas tada, kai reikia bendros architektūrinės vizijos
    - Siekia įgyvendinti kokybinius atributus sutarta svarbos tvarka
    - Taikomas tiek, kol projektinės rizikos sumažėja iki norimo lygio
    - Sprendimai aprašomi sutartu standartu
    - Taktiniai sprendimai paliekami emergent design projektavimui

<br>

- *Didelėms, sudėtingoms, rizikingoms sistemoms realizuoti būtinas išankstinis planas.*
1. Geras detalus apgalvojimas projektavimo fazėje tekainuoja kelias valandas, bet gali sutaupyti mėnesius.
2. Sistemos dizainą (brėžinius, specifikacijas) lengviau suvokti ir analizuoti negu patį kodą.
3. Labiausiai patyrę profesionalai turėtų projektuoti sistemą, o jaunesni – tiksliai realizuoti pagal specifikacijas
- **YAGNI** - *you aren't gonna need it* - Žmonės yra linkę apsidrausti nuo galimų nemalonumų, todėl projektuodami informacines sistemas siekia pridėti **galimai** ateityje praversiančių savybių.

<br>

- Išankstinis planavimas reikalingas, tačiau detalus išankstinis planavimas **labiau kenkia nei padeda**. (Agile projektai sėkmingesni)
- Išankstinis projektavimas turi siekti **svarbiausių, kritinių kokybių užtikrinimo**.
- **Nekritinės** sistemos dalys geriausiai pavyksta emergent projektavimo būdu.

### Kodo organizavimo lygiai

- **Paprogramė, funkcija, procedūra, metodas** - Programos dalis - programinių instrukcijų seka, turinti identifikatorių, kuriuo ją galima iškviesti. 
- **Klasė** - „aprašas objektams gaminti“. Abstrakcija, susidedanti iš būsenos ir elgsenos (metodų).
- **Objektas** – „klasės egzempliorius“
- Moduliai ir komponentai yra „sistemos elementai su aiškia apibrėžta riba ir interfeisu“.
- **Modulis** – tai, ką projektuojame; pvz., klasė, klasių grupė, sluoksnis ir t.t.
- **Komponentas** – tai, ką analizuojame programos veikimo metu; gali būti ir trečių šalių produktas.
- **Package** - Organizuoja elementus į grupes (patogumo dėlei).
- **Node** - Fizinė arba virtuali mašina, kurioje veikia programa arba jos dalis. Turi turėti bent procesorių ir atminties resursus.
- **Layer** - Izoliuota kodo grupė, pasiekiama tik per tam tikrą interfeisą. „aukštesnis“ sluoksnis gali naudoti tik „žemesnį“ sluoksnį.
- **Tier** - Sistemos išdalinimo mechanizmas, kur skirtingos dalys gali veikti skirtingose platformose ir net naudoti skirtingas technologijas.

### Projektavimo abstrakcijų lygiai

- **Idioma** - Plačiai pripažįstama ir naudojama konvencija konkrečioje programavimo kalboje.
- **Design pattern** - Bendras, gerai žinomas sprendimas 
konkrečiai problemai 
tam tikrame kontekste.
- **Framework** - Konkrečiai probleminei sričiai sukurtas klasių rinkinys (realizuotas dalinis sprendimas), skirtas naudojimui / pritaikymui.
- **Plėtros taškai** (Extension Points) – projekte numatytos vietos pridėti papildomiems elementams.
- **Architectural pattern** - Daug platesnė apimtis nei projektavimo šablonai; paprastai realizuojama naudojant daug komponentų.
- Tipinės architektūros (**Reference architectures**) - „Žanras“, t.y., Visuotinai žinomas visos architektūros šablonas.
- **Projektavimo abstrakcijos lygiai:**
    1. Tipinės architektūros (bendras visos sistemos apibūdinimas).
    1. Architektūriniai šablonai (aukšto lygio struktūriniai sprendimai).
    1. Stambiausios komponentų grupės (sluoksniai, platformos, karkasai, bibliotekos).
    1. Abstrakcijų grupės (moduliai / komponentai). 
    1. Abstrakcijos bei algoritmai, išreikšti objektų tarpusavio sąveikomis. Projektavimo šablonai.
    1. Elgsenos grupavimas į abstrakcijas (funkcionalumas, enkapsuliuojamas klasėse)
    1. Algoritmai - išreiškiami programavimo kalba, pritaikomos idiomos

### Architekturos vertinimas

- **Gera arch** - Tokia, kuri užtikrina norimas sistemos kokybes:
    - palengvina iškeltų tikslų pasiekimą
    - įgalina ateities evoliuciją numatytomis kryptimis 
- **Bloga arch** - Tokia, kuri neužtikrina norimų sistemos kokybių:
    - apsunkina iškeltų tikslų pasiekimą
    - apsunkina ateities evoliuciją numatytomis kryptimis 
- Priežastys:
    - Nereikalingas funkcionalumas
    - Perteklinis sudėtingumas
- *Kaip mažiausiomis pastangomis pasiekti “gerą architektūrą”?*
    - Tipinės architektūros - žinomas, pasiteisinęs bendras sprendimas visai sistemai
    - Architektūriniai šablonai - žinomi, pasiteisinę aukšto lygio struktūriniai sprendiniai
    - Projektavimo šablonai - žinomi, pasiteisinę klasių lygio struktūriniai sprendiniai
    - Švarus kodas, OOP koncepcijos, OOD principai - kodo lygio organizavimo sprendimai
    - Asmeninė patirtis

### Išankstinis sistemų projektavimas

- **Pradžia**:
    - Verslo misija ir esminis poreikis. *Svarbiausios prielaidos*
    - Ribojimai:
        - Architektūriškai svarbūs reikalavimai (Architecturally significant requirements, ASR)
        - Nefunkciniai reikalavimai
        - Patys svarbiausi funkciniai reikalavimai
    - Prioritetai
- **Kompromisai:** iekvieno kokybinio atributo realizavimas neigiamai veikia kitus kokybinius atributus.
- **Kokybiniai atributai** privalo būti išmatuojami ir tiksliai skaitiškai įvertinami

### Projektinių sprendimų dokumentavimas

- Dokumentacijos **tikslai**:
    - Komunikacinis: fiksuojami susitarimai tarp šalių (programuotojų, testuotojų, skirtingų komandų ir t.t.)
    - Edukacinis: naujų projekto narių supažindinimui su sistema 
    - Sistemos architektūros analizei.
- Dokumentacijos **problemos**:
    - Atotrūkis nuo tikro, veikiančio kodo
    - Atsilikimas laikui bėgant (labai trumpas “galiojimo laikas”)
- *Svarbu vadovautis pragmatiškumo principu: ar dokumentavimo pastangos atsipirks?*
- Dokumentavimo perspektyvos (**viewtypes**):
    - Programinių modulių aprašymai (implementation time)
    - Sistemos elementų elgsenos ir interakcijų aprašymai (runtime; component-and-connectors)
    - Diegimo aprašymai (allocation)

---

## Agile

---

## Sluoksnių architektūra

---

## Modern Front End applications

---

## Nuolatinis integravimas

---