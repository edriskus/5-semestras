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

---

## Kodo testavimas (Unit Testing) ir jo įtaka projektavimui

---

## Išankstinis projektavimas (upfront design)

---

## Agile

---

## Sluoksnių architektūra

---

## Modern Front End applications

---

## Nuolatinis integravimas

---