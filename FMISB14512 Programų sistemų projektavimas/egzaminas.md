# Programų sistemų projektavimas

- [Įvadas](#%C4%AFvadas)
- [Kokybiškas programinis kodas. Refaktoringas](#kokybi%C5%A1kas-programinis-kodas-refaktoringas)
    - [Kaip programinio kodo kokybė siejasi su dizaino kokybe?](#kaip-programinio-kodo-kokyb%C4%97-siejasi-su-dizaino-kokybe)
    - [Švaraus programinio kodo taisyklės](#%C5%A1varaus-programinio-kodo-taisykl%C4%97s)
- [Objektiškai orientuoto projektavimo principai](#objekti%C5%A1kai-orientuoto-projektavimo-principai)
    - [Pagrindinės OO koncepcijos](#pagrindin%C4%97s-oo-koncepcijos)
    - [Pagrindinės OO metrikos](#pagrindin%C4%97s-oo-metrikos)
    - [Pagrindiniai OOD principai (SOLID)](#pagrindiniai-ood-principai-solid)
- [Projektavimo šablonai (Design Patterns)](#projektavimo-%C5%A1ablonai-design-patterns)
- [Kodo testavimas (Unit Testing) ir jo įtaka projektavimui](#kodo-testavimas-unit-testing-ir-jo-%C4%AFtaka-projektavimui)
    - [Unit testing](#unit-testing)
    - [Projektavimo įtaka testuojamumui / Testavimo įtaka projektavimui](#projektavimo-%C4%AFtaka-testuojamumui-testavimo-%C4%AFtaka-projektavimui)
    - [Mocking](#mocking)
    - [TDD](#tdd)
- [Išankstinis projektavimas (upfront design)](#i%C5%A1ankstinis-projektavimas-upfront-design)
    - [Kodo organizavimo lygiai](#kodo-organizavimo-lygiai)
    - [Projektavimo abstrakcijų lygiai](#projektavimo-abstrakcij%C5%B3-lygiai)
    - [Architekturos vertinimas](#architekturos-vertinimas)
    - [Išankstinis sistemų projektavimas](#i%C5%A1ankstinis-sistem%C5%B3-projektavimas)
    - [Projektinių sprendimų dokumentavimas](#projektini%C5%B3-sprendim%C5%B3-dokumentavimas)
- [Agile](#agile)
    - [Extreme Programming (XP)](#extreme-programming-xp)
    - [Scrum](#scrum)
- [Sluoksnių architektūra](#sluoksni%C5%B3-architekt%C5%ABra)
- [Modern Front End applications](#modern-front-end-applications)
- [Nuolatinis integravimas](#nuolatinis-integravimas)
    - [CI](#ci)
    - [CD](#cd)

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

- **Agile software development** describes a set of values and principles for software development under which requirements and solutions evolve through the collaborative effort of self-organizing cross-functional teams.[1] It advocates adaptive planning, evolutionary development, early delivery, and continuous improvement, and it encourages rapid and flexible response to change.[2] These principles support the definition and continuing evolution of many software development methods.[3]
- **MVP (Minimum Viable Product)** - Minimalus produktas, kuriuo bandoma patikrinti ar identifikuota problema verta sprendimo.
- **RAT (Riskiest Assumption Test)** - Siekiama per kuo mažiau laiko išmokti kuo daugiau.

### Extreme Programming (XP)

- The methodology takes its name from the idea that the beneficial elements of traditional software engineering practices are taken to "extreme" levels. As an example, code reviews are considered a beneficial practice; taken to the extreme, code can be reviewed continuously, i.e. the practice of pair programming.

### Scrum


- Dokumentas, kuriame kaupiamos prioritetizuotos nepradėtos bei nebaigtos užduotys (living backlog).
- Sprintai – trumpos iteracijos, kurių kiekvienoje užbaigiamos kelios stambios užduotys.
- Kasdieniai scrumai – trumpi susirinkimai, kurių metu kiekvienas grupės narys pasako, ką per dieną nuveikė ir ką planuoja daryti toliau.
- Planavimo sesijos, kurių metu išrenkamos užduotys tolesniam sprintui ir priskiriamos grupės nariams.
- Sprinto pabaigos susitikimai, kurių metu analizuojama praėjusio sprinto patirtis, kaip ir kodėl užduočių laiko įverčiai skyrėsi nuo realiai prie jų praleisto laiko.
- Sprinto demonstracijos, kurių metu komandos viena kitai parodo pasibaigusio sprinto metu užbaigtas dalis.

<br>

- **Product owner** - produkto savininkas. Siekia sukurti kuo daugiau vertės.
- **Development team** - kūrėjų (programuotojų, testuotojų, dizainerių) komanda. Siekia kurti produktą išvengdami perteklinio darbo.
- **Scrum master** - scrum proceso meistas. Siekia, kad komanda su kuo mažiau vargo sukurtų kuo daugiau vertės.

---

## Sluoksnių architektūra

- Principai:
    - Interesų atskirimas skirtinguose sluoksniuose (separation of concerns)
        - Sluoksnyje saugoma logika skirta tik tam sluoksniui
    - Sluoksnių izoliacija (layers of isolation)
        - Įgalina refaktorinimą sluoksniuose
        - Sliuoksnių implementacijos keitimą (prezentacijos bibliotekos / duombazes keitimas)
- **Dalykinės srities modelis** - organizuotas ir struktūrizuotas problemos suvokimas/žinojimas

<br>


- **DAO** (Data Access Object) - Objektas, kuris suteikia abstrakčią sąsaja komunikuoti su duomenų baze ar kitu duomenų saugojimo mechanizmu (pvz. failu). Leidžiamos duomenų operacijos nesigilinant į duomenų saugojimo detales.
- Privalumai:
    - Verslo logikos atskyrimas nuo duomenų saugojimo.
    - Patogu naudoti kartu su ORM karkasais
    - Pernaudojamos CRUD operacijos igyvendinant abstraktų DAO.
- Trūkumai:
    - Kodo dublikacija
    - Nėra motyvacijos parašyti naują efektyvesnę užklausą, jei galima lengvai perpanaudoti vieną ar kelias esamas. 
    - Operavimas “pilnais” objektais, net kai to visai nereikia.

<br>

- **ORM** (Object Relational Mapping) - ORM programavimo technika, skirta konvertuoti duomenis tarp reliacinės duomenų bazės ir objektiškai orientuotos programavimo kalbos.
- Privalumai:
    - Įgalina įgyvendinti domain model šabloną.
    - Sumažėja daug kodo, skirto darbui su DB.
    - Pakeitimai objektų modelyje atliekami vienoje vietoje.
    - Objektiškai orientuotos užklausos.
    - Navigacija - susiję įrašai užkraunami to pareikalavus kode.
    - Palaikoma lygiagretumas, duomenų kešavimas (cache) ir transakcijų valdymas.
- Trūkumai:
    - Nukenčia sparta
    - Mažesnis dėmesys į SQL užklausas
    - Tam tikras sudėtingumas norint parašyti labai sudėtingas užklausas

<br>

- Strategijos, kaip suvaldyti lygiagrečius procesus - **“concurrency”**.
- **Optimistinis rakinimas** - gerai, kai tikimybė, jog tai nutiks yra nedidelė.
Reikėtų pasirūpinti, kad nenukentėtų vartotojo lūkesčiai
- **Pesimistinis rakinimas** - tinka, kai tikimybė, jog tai nutiks yra didelė. Didelė tikimybė, kad nukentės aplikacijos sparta
- **Paskutinė užklausa laimi.** Reikia suprasti reikalavimus ir vartotojų lūkesčius.

<br>

- **MVC** (Model View Controller)
- Modelis (**model**) - objektas, kuris atspindi tam tikrą domeno informaciją (Domain model, Transaction script). 
- Atvaizdavimas (**View**) - skirtas modelio duomenų atvaizdavimui vartotojo sąsajoje. 
- Valdytojas (**Controller**) - priima vartotojo įvestą informaciją, perduoda ją modeliui ir grąžina apdorotus duomenis atgal atvaizdavimui.

<br>

- **REST** - HTTP paremtas API
    - Atskirti klientas ir serveris (duomenų atvaizdavimas vs išsaugojimas)
    - Vienoda sąsaja
    - Būsenos eliminacija tarp sąveikų
    - Kešuojamas
    - Sluoksniuota sistema

---

## Modern Front End applications

- **HTML5:**
    - Geolocation
    - Drag and Drop
    - Local Storage
    - Application Cache
    - Web Workers
    - SSE
    - Service Workers
- **CSS3:**
    - Animations
    - Gradients
    - Flexbox
    - Box-model
    - Media queries
- **SASS:**
    - Variables
    - Nesting
    - Extend/inherit
- **EcmaScript**
    - 2009m. (ES5)
    - ES2015 (ES6) - class, iterator, generator, arrow fn, modules, etc..
    - ES2016 (ES7)
    - ES8+++
- **Node v9+**
- **Angular 5.2.0** - TypeScript
- **Vue** - JS
- **React** - JSX
- **Testing:**
    - Mocha
    - Karma
    - Jasmine
- **Building**
    - Rollup
    - Browserify
    - Webpack

---

## Nuolatinis integravimas

- Continuous Integration / Continuous Delivery
- **Anti-patterns**:
    - Programinės įrangos diegimas **rankiniu būdu**
        - Nepastovios diegimo procedūros
        - Dažni versijų atstatymai (roll back) 
        - Po išleidimo nebeveikianti produkcijos (production) aplinka
        - Priklausomybė nuo kelių žmonių, žinančių diegimo procesą
        - Žmogiškos klaidos: kažkas pamirštama padaryti ar padaroma klaidingai 
    - Diegimas vykdomas tik **pabaigus visus programavimo darbus**
        - Pasiliekama per mažai laiko susikonfiguruoti aplinkas ir procesus
        - Testavimas daromas lokaliuose kompiuteriuose
        - Dažnai produkcijos aplinka skiriasi nuo lokalius, dėl ko per vėlai pamatomos įgyvendinimo klaidos. Kuo ilgiau tęsiasi programavimas, tuo būna sunkiau jas pataisyti. 
        - Pvz. programuojama ant Windows OS, o diegiama į Linux serverius (failų pavadinimų klaidos)
    - Rankinis **konfigūracijų tvarkymas**
        - Sistema tinkamai veikia testinėje aplinkoje, bet “lūžta” produkcinėje
        - Sistemų administratoriai ilgai užtrunka paruošti naują aplinką
        - Negalima lengvai grįžti prie anksčiau veikusios konfigūracijos

### CI

- Tai tokia programavimo praktika, kurios metu stengiamasi integruoti komandos parašytą kodą kaip įmanoma dažniau (bent kelis kartus per dieną)
- Reikalavimai
    - Reguliarus kodo įtraukimas į versijavimo sistemą ( Check in regularly)
    - Visapusiškai automatizuoti testai
    - Trumpas konstravimo ir testavimo procesas
    - Tvarkinga programavimo aplinka
- Esminės **praktikos:**
    - Neįtraukti neveikiančio / konstravimą griaunančio kodo
    - Prieš einant namo nepalikti neveikiančio build’o
    - Automatizuotų testų leidimas prieš įtraukiant kodą į versijavimo repozitoriją
    - Prieš pradedant naują užduotį palaukti, kol baigs vykdytis testų leidimas
    - Limituoti laiką, skirtą sistemos sutvarkymui prieš revizijos atstatymą
    - Visą laiką būti pasiruošusiam grąžinti sistemą į prieš tai buvusią reviziją
    - Neišjunginėti neveikiančių testų
    - Prisiimti atsakomybę dėl nugriautos sistemos
    - Test driven development (TDD)
- **Įrankiai:**
    - Teamcity
    - Jenkins
    - Team Foundation Server
    - Bamboo
    - Travis
- **Deployment pipeline** - Etapais išskaidytas ir automatizuotas procesas, kuris padeda kodui pasiekti galutinį vartotoją
- Etapai:
    - **Kodo** - įvertina, kad sistema veikia techniniam lygmenyje (kompiliuojasi, veikia unit testai, praeina statinę analizę)
    - **Automatizuotų priėmimo (acceptance) testų** - įvertina, kad sistema veikia funkciniame ir nefunkciniame lygmenyje, veikimas atitinka reikalavimus
    - **Rankinio testavimo** - įvertinama sistemos naudingumas vartotojui, patikrinama, ar nėra klaidų, kurios buvo nerastos automatizuotų testų
    - **Išleidimo** - sistema įdiegama į vartotojui pasiekiamą aplinką
- **Building** - programinės įrangos konstravimas
- **Build tools:** Make (c), Apache Ant (any language), Gradle (Java, Android), Apache Maven (Java), MSBuild (.net), Rake (ruby), Phing (php), Grunt (Javascript), Gulp (Javascript)
- **Tipiniai veiksmai**:
    - Kodo kompiliavimas
    - Failų / resursų paruošimas
    - Pakavimas
    - Konfigūracijos paruošimas
    - Automatizuotų testų leidimas

### CD

- *Programinės įrangos kūrimo principas, kurio metu komandos kuria vertingą funkcionalumą trumpomis iteracijomis ir jis iškart gali būti išleistas vartotojams*
- Nauda:
    - Pagreitinti išleidimai
    - Greitesnis grįžtamasis ryšys iš vartotojų
    - Mažiau klaidų, o esamos ištaisomos greičiau
- **Funkcionalumo jungikliai** (Feature Toggling): Skirti įjungti / išjungti tam tikrą funkcionalumą
- **Monitoring**: 
    - Automatiniai įspėjimai (email, sms, chat, dashboard)
    - Anomalijų radimas
    - Profiliavimas (profiling)
    - Vartotojų stebėsena (google analytics)
- Vienas populersnių įrankių šiai dienai ELK (elastic + logstash + kibana)


---