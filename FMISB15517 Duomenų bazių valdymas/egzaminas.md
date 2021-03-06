# FMISB15517 Duomenų bazių valdymas

0. [Duomenų bazių valdymo sistemos](#duomenų-bazių-valdymo-sistemos)
1. [SQL užklausų apdorojimas](#sql-užklausų-apdorojimas)
2. [Išskirstytos duomenų bazės](#išskirstytos-duomenų-bazės)
3. [Transakcijos](#transakcijos)
4. [Duomenų bazės fiziniame lygmenyje](#duomenų-bazės-fiziniame-lygmenyje)
5. [Duomenų bazės įrašai](#duomenų-bazės-įrašai)
6. [Indeksai](#indeksai)
7. [Objektiškai orientuotos ir objektinės DB](#objektiškai-orientuotos-ir-objektinės-db)
8. [Dedukcinės DB](#dedukcinės-db)
9. [Aktyviosios DB](#aktyviosios-db)

## [Duomenų bazių valdymo sistemos](#duomenų-bazių-valdymo-sistemos)
- **Duomenų bazių valdymo sistema (DBVS)** - programinių priemonių kompleksas, leidžiantis:
    - Aprašyti (sukurti) duomenų struktūras
    - Įvesti duomenis (aktualaus būvio palaikymas)
    - Atlikti duomenų paieška (svarbiausia funkcija)
    - Sudaryti įvairias ataskaitas
    - *Taip pat: Duomenų vientisumo palaikymas, Duomenų neprieštaringumo kontrolė, Duomenų apsauga, Kelių nepriklausomai dirbančių vartotojų vienalaikis darbas su duomenimis, Duomenų bazės būsenos atstatymas po avarijų*
- **Kliento - serverio DB architektūra**:
    - **DB serveris** yra atsakingas už: duomenų keitimo ir išgavimo apdorojimą, duomenų apdorojimą, DB taisyklių ir ribojimų įgyvendinimą ir palaikymą, duomenų saugumo palaikymą.
    - **DB klientas** yra atsakingas už: duomenų pateikimą vartotojui, sąsajos užtikrinimą, užklausų perdavimą serveriui.
    - Dažnai naudojamos trijų ir daugiau sluoksnių kliento ir serverio architektūros, kur verslo logika, įgyvendinama verslo taisyklių pagalba, yra apdorojama atskiro serverio.
    - Ryšys tarp kliento ir serverio standartiškai užtikrinamas naudojant **Open Database Connectivity (ODBC)**
    - **3 sluoksnių arch**: FE - BE - DB
- **Populiariausios DBMS:**
    1. Oracle *RDBMS*
    2. MySQL *RDBMS*
    3. Microsoft SQL Server *RDBMS*
    4. PostgreSQL *RDBMS*
    5. MongoDB *DS*
- **Palyginimas:** (1-4)
    - **Visos** turi Union, Inner/Outer joins, inner selects, merges, BLOBs and CLOBs, kursorius, trigerius, funkcijas, procedūras, išorines paprogrames.
    - Microsoft SLQ (nuo 2005), Oracle ir PostgreSQL turi **Intersect ir Except**, MySQL ir Informix (?) neturi.
- **Kiekybinės charakteristikos:**
    - **Max DB dydis**
    <br>*Dažniausiai neribotas arba labai didelis (500TB Microsoft SQL)*

    - **Max lentelės dydis**
    <br>*Oracle 4GB, MySQL 2-16GB, PostgreSQL 32TB, MSSQL 500TB*

    - **Max eilutės dydis**
    <br>*MSSQL ir MySQL kilobaitai, PostgreSQL 1.6TB, Oracle neribotas*

    - **Max stulpelių skaičius**
    <br>*200-3000 įvairiai*

    - **Max BLOB/CLOB dydis**
    <br>*1-4GB įvairiai*

    - **Max CHAR dydis**
    <br>*Kilobaitai, tik PostgreSQL - 1GB*

    - **Max NUMBER dydis**
    <br>*Bitai, tik PostgreSQL - Neribotas*

### NoSQL

- Reliacinė bazė, nenaudojanti SQL kalbos (no relational). Taip pat suprantama kaip **not only SQL**:
    - **Raktas-reikšmė**
    <br>*Raktas - string, reikšmė - bet kas. Galimos **GET**, **PUT** bei **DELETE** operacijos. Greitos ir lanksčios.*
    <br>Redis, Riak
    
    - **Grafų** (tinklinės)
    <br>*Susideda iš mazgų, ryšių tarp jų ir mazgų bei ryšių savybių. Duomenų grafai nėra struktūrizuoti, nėra schemų ir nėra privaloma apibūdinti duomenų tipus*
    <br>Neo4j, FlockDB
    
    - **Stulpelio** tipo
    <br>*Dar vadinamos **vertikaliosiomis**. Kaip RDB, tik orientuotos į stulpelius. Lengva suspausti, atlikti agregacinius veiksmus. Lėtesnis įrašų skaitymas ir rašymas.*
    <br>HBase, Cassandra
    
    - **Dokumentinės** (į dokumentus orientuotos)
    <br>*Duomenys saugomi kolekcijose. Dokumento formatas ir schema nėra iš anksto aprašomi. Galimi hierarchiniai duomenys (nested)*
    <br>MongoDB, CoucheDB
    
    - **Multi-modelinės**
    <br>*Anksčiau minėtų duomenų bazių naudojimas kartu (įvairus deriniai). Tam tikrais atvejais kartu naudojant kelias skirtingas DBVS sudaroma lankstesnė ir patikimesnė sistema. Pavyzdžiui, socialinių tinklų projektai dažnai naudoja reliacinę arba stulpelio tipo duomenų bazę kartu su grafų duomenų baze.*

### ACID

- **Atomiškumas** (angl. Atomicity) – transakcija vykdoma kaip atomarinė operacija, t.y. arba visa vykdoma arba visa nevykdoma;
- **Stabilumas** (angl. Consistency) – tiek prieš transakciją, tiek ir po jos sistema yra normalioje darbo būsenoje;
- **Izoliacija** (angl. Isolation) – skirtingų vartotojų transakcijos neturi trukdyti viena kitai;
- **Ilgalaikiškumas/Negrįžtamumas** (angl. Durability) – jei transakcija įvykdyta, tai jos darbo rezultatas turi būti saugomas DB.

### BASE

*ACID reikalavimų įgyvendinimas reikalauja laiko ir sistemos resursų. Kai kuriose NoSQL sistemoms buvo įgyvendinti lankstesni BASE reikalavimai:*
- **Bazinis prieinamumas** (angl. basic availability) – kiekviena užklausa garantuotai pasibaigs (sėkmingai arba nesėkmingai);
- **Lanksti būsena** (angl. soft state) – sistemos būsena gali keistis laikui bėgant;
- **Galutinis neprieštaringumas** (angl. eventual consistency) – duomenys gali būti nedarnūs tam tikru laiko momentu, bet galiausiai jie taps darnūs.

<hr>

## [SQL užklausų apdorojimas](#sql-užklausų-apdorojimas)

- **Sistemos katalogas** - objektų rinkinys, saugantis informaciją apie kitus DB esančius objektus ir DB struktūrą.

### Užklausos vykdymo etapai

1. SQL sakinys
    - Sakinio sintaksinė analizė
    - Semantinė analizė **(DBSK)**
2. Vidinė užklausos forma (medis, grafas)
    - Optimizacija **(DBSK)**
    - Plano sudarymas
3. Vykdymo planas
    - Plano vykdymas
    4. Sakinio rezultatas  

*Užklausos vykdymas DB procesoriumi **(runtime DB processor)** gali būti nesėkmingas. Tada vietoj rezultato procesorius pateikia klaidos pranešimą.*

- **Užklausų optimizacijos** tikslas - nustatyti visiškai optimalų užklausos vykdymo kelią.
- Optimizavimas gali užtrukti ilgiau, nei vykdymas, todėl naudojama **dalinė, statistinė** optimizacija.
- Kiekviena DBVS turi savo komplektą pagrindinių `SELECT` ir `JOIN` operacijų, kuriomis gali būti optimizuota SQL užklausa.
<br><br>
- Neoptimizavus medžio struktūros, pradinis medis visada atitiks užklausos (pvz., SQL kalba) logiką.
- Kanoninė struktūra atrodo taip: pirmiausia atliekama Dekarto sandauga, tada išrinkimas pagal apjungimo ir kitas salygas, tada projekcija.
- Tokios užklausos vykdymas pažodžiui nebus efektyvus dėl didelės, kartais dvigubos-trigubos (priklausomai nuo dalyvaujančių lentelių skaičiaus) Dekarto sandaugos.
- Vykdant medžio struktūros transformacijas, svarbu įsitikinti, kad restruktūrizacijos procesas nepakeis galutinio rezultato.

### Pagrindinės transformavimo taisyklės

 1. σ<sub> C1 AND C2 AND C3 ...</sub>(R) ≡ σ<sub>C1</sub>(σ<sub>C2</sub>(σ<sub>C3</sub>...(R)...))
 <br>*Konjunktyvus išrinkimas skaidomas į seriją paprastų išrinkimų.*

 2. σ<sub>C1</sub>(σ<sub>C2</sub>(R)) ≡ σ<sub>C2</sub>(σ<sub>C1</sub>(R))
 <br>*Komutatyvumas.*

 3. π<sub>S1</sub>(π<sub>S2</sub>(π<sub>S3</sub>...(R)...)) ≡ π<sub>S1</sub>(R)
 <br>*Projekcijos kaskadiškumas.*

 4. π<sub>S1</sub>(σ<sub>C1</sub>(R)) ≡ σ<sub>C1</sub>(π<sub>S1</sub>(R))
 <br>*Išrinkimo ir projekcijos komutatyvumas.*

 5. R ⋈<sub>C</sub> S ≡ S ⋈<sub>C</sub> R
 <br>*Dekarto sandaugos (arba **Join**) komutatyvumas.*

 6. σ<sub>C</sub>(R ⋈ S) ≡ (σ<sub>C</sub>(R)) ⋈ S
 <br>σ<sub>C</sub>(R ⋈ S) ≡ (σ<sub>C1</sub>(R)) ⋈ (σ<sub>C2</sub>(S))
 <br>*Išrinkimo ir Dekarto sandaugos (arba **Join**) komutatyvumas.*

 7. π<sub>L</sub>(R ⋈<sub>C</sub> S) ≡ (π<sub>A1,...,An</sub>(R)) ⋈<sub>C</sub> (π<sub>B1,...,Bm</sub>(R))
 <br>*Projekcijos ir Dekarto sandaugos (arba **Join**) komutatyvumas.*
 <br>***Jei sąlygos C laukai nėra L sąraše**, jie turi būti įtraukti į pirmines projekcijas, o atlikus JOIN reiks suprojektuoti papildomai be C. Dekarto sandaugos atveju tai nėra aktualu (C nėra).*

 8. Operacijos su ∪ ir ∩ komutatyvios, bet su - ne.
 9. (R * S) * T ≡ R * (S * T)
 <br>*Dekarto sandauga (arba **Join**) bei ∪ ir ∩ atskirai kiekviena yra asociatyvios.*

10. σ<sub>C</sub> (R * S) ≡ (σ<sub>C</sub> (R)) * (σ<sub>C</sub> (S))
 <br>*Išrinkimas komutuoja su ∪ ir ∩ ir -*

11. π<sub>C</sub> (R ∪ S) ≡ (π<sub>C</sub> (R)) ∪ (π<sub>C</sub> (S))
 <br>*Projekcija komutuoja su ∪*

12. C ≡ NOT(C1 AND C2) ≡ NOT(C1) OR NOT(C2)
<br>C ≡ NOT(C1 OR C2) ≡ NOT(C1) AND NOT(C2)
 <br>*DeMorgano (išrinkimo sąlygai C)*

### Siūlomi optimizacijos algoritmo žingsniai

1. Pagal 1 taisyklę vieną SELECT išskaidyti į kelias – bus daugiau laisvės.
2. Pagal 2, 4, 6, 10 (SELECT komutatyvumas) SELECT nuleisti medžiu kaip galima žemyn (į lapus)
3. Pagal 9 pastumti griežčiausią SELECT (duodančią mažiausią rezultatą pagal įrašus ar bendrą dydį) kuo arčiau vykdymo pradžios.
4. X derinti su išrinkimo sąlyga (kad atitiktu JOIN)
5. Pagal 3, 4, 7, 11 nustumti projekcijos kintamuosius kaip galima arčiau vykdymo pradžios (žemyn į lapus)
6. Išskirti šakų grupes, kurios galėtų būti vykdomos vienu algoritmu.

---

### Reliacinė algebra
<a name="cht-reliacine-algebra"></a>
- Tai teorinė kalba, skirta darbui su vienu arba keliais sąryšiais.
- Reliacinės algebros taikymo rezultatas – naujas sąryšis.
- Sukurta E.F.Codd’o 1972 metais.

#### Operacijos:
- Išrinkimas (**Selection**) *Išrenka eilutes, atitinkančias sąlygą*<br>
`SELECT * FROM relation WHERE condition;`
> σ<sub>condition</sub>(relation)

- Projekcija (**Projection**) *Išrenka tik nurodytus stulpelius*<br>
`SELECT attr1, attr2, attr3 FROM relation;`
> π<sub>list of attributes</sub>(relation)

- Dekarto sandauga (**Cartesian product**) *Aibė visų įmanomų eilučių porų*<br>
`SELECT Employee.*, Customer.* FROM Employee, Customer;`
> R x S

- Sąjunga (**Union**) *Sujungia visas eilutes į vieną sąryšį, pašalinant dublikatus.*<br>
`SELECT * FROM Employee1 UNION SELECT * FROM Employee2;`
> R υ S

- Skirtumas (**Difference**) *Sudaro sąryšį iš eilučių, kurios yra pirmame sąryšyje ir kurių nėra antrame.*<br>
`SELECT * FROM Employee1 EXCEPT SELECT * FROM Employee2;`
> R - S

- Sankirta (**Intersection**) *Sukuriamas naujas sąryšis iš bendrų pirmo ir antro sąryšio eilučių.*<br>
`SELECT * FROM Employee1 INTERSECT (SELECT * FROM Employee2);`
> R ⋂ S

- Jungtis (**Join**) *visos įmanomos poros iš jungiamų sąryšių, tenkinančios tam tikrą sąlygą. Tai - **išvestinė operacija***
    - Theta-join
    > R ⋈<sub>F</sub> S = σ<sub>F</sub> (R x S)

    - Equi-join *⋈, Kai F yra =*
    - Natural join
    > R ⋈ S

    - Outer Join (LEFT | RIGHT) *Disjunkcija*
    - Inner Join *Konjunkcija*

<hr>

## [Išskirstytos duomenų bazės](#išskirstytos-duomenų-bazės)

- **Išskirstytoji duomenų bazė** – tai duomenų rinkinys, logiškai priklausantis tai pačiai duomenų bazei, bet fiziškai saugomas skirtingose vietose, t.y. mazguose, sujungtuose kompiuterių tinklu
- **Šliuzai** (Gateways) – duomenų mainams paruošiamos nesudėtingos procedūros, nėra tarp šaltinių sąveikos kaip vieningoje DBVS
- Duomenys išskirstomi į **fragmentus** tam, kad juos galima būtų išskirstyti tarp mazgų taip, kad viena užklausa būtų vykdoma viename mazge.
- **Fragmentacijos schema** aprašo, kokiu būdu turėtų būti gaunamas bendras DB vaizdas iš atskirų fragmentų.
- **Dislokacijos schema** nusako, kokiuose tinklo mazguose yra išdėstyti minimi DB fragmentai.
- **Replikacija** – duomenų atkartojimas kitame serveryje. 
- **Pilnoji** replikacija – visa duomenų bazė, atkartota visuose serveriuose (ribinis atvejis ).
    - Patikimumas ir prieinamumas stipriai išauga, tačiau darbas lėtas, sudėtingi konkurencinės kontrolės ir DB atstatymo mechanizmai.
    - Tarpiniai atvejai – dalinė replikacija.
- **Replikacijos schema** nusako, kiek, kokiuose tinklo mazguose, kokių fragmentų kopijos daromos.

***Naudotojui išskirstyta sistema turi atrodyti taip pat, kaip ir neišskirstytoji sistema!***

- Nepriklausymas nuo **fragmentavimo**
- Nepriklausymas nuo **replikavimo**;
- Nepriklausymas nuo **operacinės sistemos**;
- Nepriklausymas nuo **tinklo**;
- Nepriklausymas nuo **DBVS**.
- **Techninis** nepriklausomumas;
- Išskirstytųjų **užklausų** apdorojimas;
- Išskirstytų **transakcijų** valdymas;
- Ekonominė nauda. *(efektyviausia, kai maži galingumai sujungiami į vieną tinklą)*

<br>

- Programinė įranga skirstoma į tris lygmenis:
    - Serverio lygmuo;
    - Kliento lygmuo;
    - Susisiekimo (ryšių) lygmuo.
- Kliento funkcijos:
    - Parengti išskirstytų užklausų vykdymo planą;
    - Prižiūrėti šio plano vykdymą, t.y. siųsti komandas serveriams;
    - Paslėpti nuo vartotojo duomenų išskirstymo detales (išskirstymo permatomumas/skaidrumas)

### Trūkumai

- Sudėtingesnė valdymo požiūriu;
- Brangesnė (ypač programinė įranga ir tinklų eksploatacija bei priežiūra);
- Jautresnė saugai (sunkiau kontroliuoti);
- Sunkiau kontroliuoti DB vientisumą;
- Sunkiau projektuojama;
- Silpniau standartizuota (ypač heterogeninės DB)

### Tipai
- **Homogeninės** (visuose mazguose veikia to paties gamintojo DBVS) – lengvai valdomos, tačiau sunkiai įgyvendinamos:
    - Autonominės
    - Neautonominės (egzistuoja centrinė koordinuojanti DBVS)
- **Heterogeninės** (mazguose veikia skirtingos kilmės DBVS) – sunkiai valdomos, dažnai paveldėtos sistemos
    - Su **pilnu** DBVS funkcionalumu (angl. full DBMS functionality)
    - Su **daliniu** DBVS funkcionalumu (angl. partial-multidatabase)
        - Federacinė – lokalios DBVS palaiko specifinių duomenų užklausas
            - *Silpnai* sukibusi (angl. loose integration) – lokalios duomenų bazės turi savo DB schemas
            - *Tampriai* sukibusi (angl. tight integration) – lokalios duomenų bazės naudoja bendrą DB schemą
        - Iš centro valdoma – visi duomenys pasiekiami per centrinį koordinuojantį modulį

### Fragmentacija
- **Horizontalioji**:
    - Išskirstomi sąryšio kortežai. Tai analogas išrinkimo **(SELECT)** operacijai.
    - Fragmentacija yra **visiška**, jei yra fragmentai, išskirstyti pagal sąlygas C1, C2, ..., Cn ir šie fragmentai apima visas sąlygas, t.y. kiekvienas įrašas tenkina bent vieną sąlygą.
    - Dažnai įrašas gali tenkinti tik vieną iš pateiktų sąlygų **(disjoint)**, pvz. Darbuotojų išskirstymas pagal skyrius.
- **Vertikalioji**:
    - Išskirstymas vyksta tarp atributų;
    - **Pirminio rakto atributas** įtraukiamas į kiekvieną fragmentą.
    - Vertikalioji fragmentacija turi atitikti šias taisykles:
        - **Užbaigtumo** – visų fragmentų aibė sudaro pradinį sąryšį;
        - **Grąžinimo taisyklė** – visų fragmentų junginys (angl. join) atitinka pradinį sąryšį;
        - **Unikalumo** – bet kurių dviejų fragmentų atributai neturi kartotis, išskyrus raktinį atributą.
- **Mišrioji** (Naudojant abi fragmentacijas)

<br>

- Savybės:
    - **Efektyvu**: duomenys saugomi ten, kur naudojami;
    - **Geresnis našumas**: lokali prieigos optimizacija;
    - **Saugumas**: vietoje saugomi tik reikalingi duomenys;
    - Užklausos surenkant duomenis **lengviau vykdomos**: naudojamas duomenų iš skirtingų skirsnių (partitions) sujungimas (union) horizontalios fragmentacijos atveju;
    <br><br>
    - Gali būti **nepriimtina prieigos sparta** kai duomenys išbarstyti;
    - **Maža duomenų sauga**: nėra duomenų pasikartojamumo;
    - Užklausos surenkant duomenis **sunkiai vykdomos**: naudojamas duomenų iš skirtingų skirsnių (partitions) apjungimas (join) vertikalios fragmentacijos atveju.

<br>

- Skirstant reikia įvertinti:
    - Užklausų kilmę ir turinį;
    - Komunikacijų sąnaudas;
    - Duomenų saugyklų dydžius;
    - Apdorojimo pajėgumas;
    - Fragmentų replikaciją.

### DB Integracijos laipsnis
- Sistema laikoma **visiškai neautonomine**, jei prie duomenų galima prieiti tik per kliento PĮ ir jos paskirstymą, net jei duomenys saugomi lokaliai.
- Sistema yra **dalinai autonominė**, jei kai kuriuos duomenis galima pasiekti tiesiogiai (lokaliomis transakcijomis).
- **Globalios integracijos** atveju vartotojui atrodo, kad jis dirba su centralizuota DB.
- Kai yra visiška autonomija, bazė vadinama **federacine** ar **multibazine** (kiekviena dalis turi savo vartotojus, ir tik kai kurie duomenys imami iš kito serverio).
<hr>

## [Transakcijos](#transakcijos)
- Kai dirbama iš karto keliomis programomis, galimi keli režimai:
    - **Pakaitinis** (procesorius vienas, dalija savo laika skirtingoms užduotims, pvz., kol vyksta skaitymas iš disko, gali dirbti kitus darbus) - **nagrinėjame šitą**
    - Vienalaikis (keli procesoriai tvarkosi tuo pat metu su skirtingomis užduotimis)
- **Transakcija** - atomiška procedūra ar užduoties dalis (gali būti įvykdyta pilnai arba neturi būti įvykdyta išvis).
- Sistema, atlikdama transakciją, garantuoja, kad:
    - Sėkmės atveju pakeisti duomenys bus negrįžtamai įrašyti
    - Nesėkmės atveju nebus įtakos kitoms transakcijoms ir duomenys liks nepaliesti

### Galimos problemos

- **Prarastas duomenų atnaujinimas**
<br>*vienas vartotojas įrašą nuskaito ir ilgai jį apdoroja, tuo tarpu kitas per tą laiką įrašą spėja atnaujinti*

- **Klaidingas sumavimas**
<br>*atliekama grupavimo funkcija, tačiau tuo tarpu atnaujinami keli įrašai. Funkcija bus suskaičiuota, panaudojant dalį atnaujintų duomenų, dalį – dar nespėtų atnaujinti*

- **Nešvarus skaitymas**
<br>*duomenys buvo įrašyti laikinai (tarpiniai), bet dėl trykio transakcija lūžo. Kitas vartotojas nuskaito duomenis, nespėjus atstatyti duomenų į pradinį (švarį) būvį, kadangi blokas su pakeistais duomenimis yra atmintyje*

- **Neatsikartojantis dvigubas skaitymas**
<br>*Jei transakcija reikalauja tą patį įrašą nuskaityti keletą kartų, o po pirmojo skaitymo kita transakcija įrašą pakeitė, tai antrą kartą nuskaitytas tas pats įrašas nebebus toks pat.*


### Lūžių priežastys

1. Aparatinės įrangos trykiai
2. Sistemos ar transakcijos trykiai *(/0, integer overflow, etc..)*
3. Lokalios klaidos *(pvz. nerasti reikalingi duomenys)* 
4. Transakcija priverstinai nutraukta *(kontrolės mechanizmas neleidžia)*
5. Kietojo disko gedimai *(prarandami atminties blokai)*
6. *Force majeure* - gamtos katastrofa, pasaulio pabaiga ir pan.

### Transakcijos vykdymas

- `BEGIN_TRANSACTION` – žymi loginę transakcijos pradžią;
- `READ` arba `WRITE` – duomenų nuskaitymas ir keitimas;
- `END_TRANSACTION` – žymi loginę transakcijos pabaigą, bet reikia dar patikrinti, kaip sėkmingai ji baigėsi, ar galima duomenis išsaugoti negrįžtamai;
- `COMMIT_TARNSACTION` – žymi sėkmingą transakcijos įvykdymą, duomenys gali būti atnaujinti negrįžtamai;
- `ROLLBACK` arba `ABORT` –žymi nesėkmingą baigtį. Reikia atstatyti pakeitimus į pradinį būseną.

<br>

- `UNDO` panaši į `ROLLBACK`, tačiau atšaukia ne visą transakciją, o tik paskutinį veiksmą;
- `REDO` nurodo, kad tam tikros operacijos turi būti atliktos pakartotinai, kad būtų galima įsitikinti sėkminga transakcijos baigtimi (visi duomenys tikrai gerai atnaujinti).

<br>

- Dalinai baigta transakcija (ang. **partially committed**) dar nebaigta, gali reikėti papildomų testų, ar tikrai viskas gerai, ar nėra interferencijos su kitomis transakcijomis.
- Transakcija užbaigta (ang. **terminated**) – tai būsena, kuri rodo, kad transakcija yra šalinama iš sistemos su kažkokia baigtimi.

### Error log

- Tam, kad avarijos atveju būtų galima atstatyti pradinį būvį, sistema veda žurnalą, kuris saugomas diske, t.y. 1- 4 trykių atvejais informacija jame lieka.
- Galimi įrašai žurnale (T – transakcijos ID):
    - \[start_transaction, T\]
    - \[write_item, T, X, old_value, new_value\] –kai pakeistas įrašas X
    - \[read_item, T, X\]
    - \[commit, T\]
    - \[abort, T\]

<hr>

## [Duomenų bazės fiziniame lygmenyje](#duomenų-bazės-fiziniame-lygmenyje)

- Laikmenų tipai:
    - **Pirminės** - greiti, riboto dydžio įrenginiai, pasiekiami CPU
    - **Antrinės** - pasiekiami tik per pirmines laikmenas
- Prieiga:
    - Nuolatinė **(online)** - *SSD, HDD, RAM*
    - Nenuolatinė **(offline)** - *Juostos, DVD, CD-ROM*
- **DB** saugomos antrinėse laikmenose:
    - DB yra didelės ir paprastai netelpa ribotose pirminėse laikmenose (operatyviojoje kompiuterio atmintyje);
    - Trykiai, lemiantys duomenų praradimą, žymiai dažniau vyksta operatyviojoje atmintyje, nei diskuose;
    - Pirminių laikmenų atminties vieneto kaina žymiai mažesnė, nei antrinių.
- **DB** susideda iš kelių failų ir daugybės įrašų, kurių tik nedidelė dalis darbo metu kopijuojama į operatyviąją atmintį ir po to apdorojus įrašoma atgal.

### Loginė disko talpos struktūra
- **Sisteminė sritis**:
    - Kelties sektorius (**Boot Sector**) – nulinio (išorinio) takelio pirmas sektorius
    - Dvi kopijos **FAT (File Allocation Table)**. Naudojama pirma kopija, antra tik atstatymui. FAT elementai būna **12** bitų (FDD) ir **16**, **32** bitų (HDD). Kuo didesnis elemento ilgis, tuo daugiau klasterių gali aptarnauti. **Klasteris** – vienas ar keli logiškai susieti sektoriai.
    - **Šakninis** (pagrindinis) katalogas. Diskeliuose gali talpinti informaciją apie 224 įrašus (išskirta 14 sektorių po 16 elementų).
- **Duomenų sritis**. Gali būti fragmentuota.

### HDD

- Didelė talpa **(250GB-4TB)**, pseudohermetiška konstrukcija.
- **Sukimosi greičiai**:
    - 5400-7200 aps/min.
    - 10000, 15000 (SCSI) aps/min.
- Galvutės nesiliečia prie paviršiaus, o juda ant oro pagalvės 0,05 – 0,1 μm atstumu.
    - Autoparkavimas (Landing Zone).
    - Aušinimas.
    - Oro filtras recirkuliacijai.
    - Duomenų buferizavimas diske (cache).
- Šiuo metu naudojama iki **1200 GB per Platter** (plokštelėje, t.y. dviejuose paviršiuose) technologija (diskų talpa kartotinė pusei šio dydžio).
- **Diskelio talpa** = Pusės * Takeliai * Sektoriai * Baitai sektoriuje
- **HDD talpa** = Cilindrai * Galvutės * Sektoriai * 512 B 
- Talpa didinama, didinant tankį (mažinant magnetinę galvutę). 2007 m. Hitachi sumažino galvutės dydį iki 50 nm.
- **Siekis** – disko galimybė pasirinkti reikalingą cilindrą.
- Vidutinė siekio trukmė (Average Seek Time) **~8 ms**. 
- **Vidutinė paieškos trukmė** – priklauso nuo galvučių mechanizmo spartos ieškant gretimo takelio.
- Patikimumas **MTBF (Mean Time Between Failures)** – apie 0,5 milijono val.
- **Spec. diskuose** magnetinės galvutės nėra judinamos, jos sumautos viena šalia kitos – po vieną kiekvienam takeliui. Brangu ir ribota.
- Laikas, reikalingas nuskaityti duomenis, susideda iš:
    - Siekio trukmės;
    - Sukimosi latentiškumo (laikas, reikalingas atsukti norimą sektorių);
    - Bloko nuskaitymo trukmės.<br>
<br>*Pirmosios dvi trukmės yra žymiai didesnės, todėl naudinga didesnius vienu metu skaitomų – rašomų duomenų masyvus talpinti tame pačiame cilindre, iš eilės einančiuose blokuose (siekis ir sukimosi latentiškumas skaičiuojamas tik pirmam blokui (sektoriui)).*

### SSD

- Spartus (greitas)
- Vartoja mažai energijos
- Atsparus mechaniškai
<br><br>
- Brangus
- Ribotas perrašymų skaičius
- MLC (Multiple Level Cell) technologija

### RAID

- Redundant Array of Independent Disks (*Perteklinis nepriklausomų diskų masyvas*) - į vieną sisemą sujungti keli diskai, programų matomi, kaip vienas. Padidina talpą ir patikimumą.
- **RAID 0**
<br>*Duomenys tolygiai paskirstyti tarp diskų, galima rašyti ar skaityti su visais diskais vienu metu. Talpa - visų diskų talpų suma. Duomenys prarandami sugedus bet kuriam diskui.*

- **RAID 1**
<br>*Visi diskai yra vienas kito kopijos. Talpa - vieno disko talpa. Duomenys prarandami TIK sugedus visiems.*

- **RAID 1E**
<br>*Bet kuri informacija saugoma bent dviejuose diskuose vienu metu. Šis jungimas nelaikomas standartiniu. Talpa - pusė visų diskų bendros talpos.*

- **RAID 5**
<br>*Visi diskai saugo po nedidelę dalį kituose diskuose esančių duomenų. Talpa = bendra - 1 diskas. Duomenys prarandami sugedus daugiau, nei 1 diskui.*

- **RAID 6**
<br>*Visi diskai saugo po nedidelę dalį kituose diskuose esančių duomenų, bet daugiau, nei RAID 5. Talpa = bendra - 2 diskai. Duomenys prarandami sugeduis daugiau, nei 2 diskams.*

<hr>

## [Duomenų bazės įrašai](#duomenų-bazės-įrašai)

- Faile talpinami duomenys, sudalinti įrašais.
- Didelės apimties objektai (binary large object) yra vadinami BLOB ir talpinami atskirose struktūrose (failuose), ne įraše. Įraše paliekama tik rodyklė į objektą. (Atitinkamai ir TXT tipas).
- Paprastai failą sudaro vieno tipo įrašai. Jie gali būti:
    - **Fiksuoto ilgio** 
    <br>*Fiksuoto ilgio įrašus lengva pasiekti: norint nuskaityti n-tajį L ilgio įrašą, reikia nuskaityti nuo (n-1)\*L+1 iki n\*L baitus.*
    - **Kintamo ilgio**
    <br>*Kai įrašo laukai kintamo ilgio, dedamas specialaus baito skirtukas*

<br>

- Disko darbo metu skaitomas ir rašomas blokas (klasteris).
    - B – bloko ilgis
    - R – įrašo ilgis
    - Kai B ≥ R, tai bfr=int(B/R), kur **bfr** vadinamas **blokavimo koeficientu** (blocking factor), o neišnaudojama vieta sudaro B-bfr\*R.
- Duomenų paskirstymas blokuose gali būti:
    - Jungtinės struktūros **(spanned)**
    - Ne jungtinės struktūros **(unspanned)**
    <br>*dažniausiai naudojama, kai B≥R ir įrašai yra fiksuoto ilgio, nes tada ypač lengva apdoroti įrašus (jie kiekviename bloke prasideda toje pačioje vietoje).*
- Kai įrašai yra kintamo ilgio, galimi naudoti abu būdai, pasirinkimą lemia vidutinis įrašo ilgis (kai jis artimas B, verta naudoti jungtinę struktūrą).
- Kintamo įrašo ilgio atveju bfr nusako vidutinis įrašo ilgis. Norint įrašyti r įrašų reiks blokų b=int(r/bfr)+1.

<hr>


## [Indeksai](#indeksai)

- **Indeksas** - duomenų struktūra, palengvinanti įrašų paiešką DB.
- **Pirminis indeksas** (primary index) - indeksas pagal įrašo raktą (reikšmė unikali).
<br><K(i), P(i)>
<br>K(i) - pirmojo lauko, esančio i-tojo bloko įraše, reikšmė
<br>P(i) - bloko adresas
<br>Indekso įrašų tiek, kiek yra blokų faile.
<br>***Block anchor*** - bloko inkaras - pirmasis bloko įrašas
- **Klasterinis indeksas** (clustering index) - nebūtinos unikalios reikšmės.
<br>*Kaip primary, tik K(i) patalpinamos visos nepasikartojančios reikšmės, o P(i) - bloko su pirmuoju rezultatu adresas.*
- **Dense index** - beveik kiekvienam įrašui yra įrašas indekse, t.y., žinome, kur yra visų raktų įrašai. Įrašai surikiuoti K(i) atžvilgiu. Užima daugiau vietos, bet paieška efektyvesnė.
- **Secondary index** - ne raktiniam laukui sukurtas indeksas:
    - Kiekvienam pasikartojančios reikšmės įrašui sukurti įrašą indekse. Porų <K(i), P(i)> skaičius bus lygus įrašų faile skaičiui.
    - Kiekvienam K(i) nurodyti daugiareikšmį <P1(i), ..., Pn(i)>. Šiuo atveju indekso įrašai bus kintamo ilgio.
    - Kiekvienai nepasikartojančiai K(i) reikšmei įrašyti porą <K(i), P(i)>, čia rodyklė P(i) rodo į bloką (papildoma struktūra), kuriame saugomos visos rodyklės į pasikartojančius su ta pačia lauko reikšme įrašus (ar jų blokus). **Naudojamas dažniausiai**

<br>

- **Daugelio lygmenų indeksai** - populiariausia įrašų paieška sutvarkytose  struktūrose, kai adresų intervalas dalijamas pusiau. Kiekvienai paieškai tarp b blokų reikia log<sub>2</sub>(b) nuskaitymo operacijų.
- Pirmasis (bazinis) lygmuo – įprastas indeksas. Kadangi struktūra sutvarkyta ir užima keletą blokų, galim pagal tą patį (raktinį) lauką sukurti pirminį indeksą indeksui.
- Įrašų skaičius antrojo lygmens indekse bus bfr kartų mažesnis. Jei ir šis užima daugiau nei bloką – antrojo lygmens indeksui galim sukurti trečiojo 
lygmens indeksą. Ir t.t.
- n-tojo (paskutinio) lygmens indeksas vad. **indekso viršūne**.
- Siekiant sumažinti problemų įterpiant ar trinant tvarkingoje lygmens struktūroje, lygmenų blokai **dinaminiuose** daugelio lygmenų indeksuose 
užpildomi nepilnai.
- Dažniausiai tokiuose indeksuose naudojama medžio struktūra.

### Paieškos medis

- Reikšmės K(i) sutvarkytos didėjimo tvarka.
- Reikšmės saugomos <P<sub>1</sub>, K<sub>1</sub>, ... , P<sub>q</sub>, K<sub>q</sub>>
- Kiekvienas P rodo į mazgą, kur K<sub>i-1</sub> < X < K<sub>i</sub>
- **Trūkumai**:
    - Algoritmai duomenims įterpti ar ištrinti negarantuoja, kad medis bus subalansuotas (visi lapai tame pačiame lygmenyje).
    - Trinant įrašus, lieka daug tuščių mazgų, o medžio struktūra didelė (lėtėja paieška).

### B tree

- Keliami papildomi reikalavimai, dėl kurių:
    - Medžiai yra subalansuoti (lapai viename lygmenyje)
    - Trynimo atveju nelieka pustuščių ar tuščių mazgų.
- Reikšmės saugomos <P<sub>1</sub>, <K<sub>1</sub>, Pr<sub>1</sub>>, ... , P<sub>q</sub>, <K<sub>q</sub>, Pr<sub>q</sub>>>
- Mazgų kiekis p/2 < q < p
- Visi lapai tame pačiame lygmenyje, rodyklės į mazgus yra NULL.
- Trinant duomenis, jei mazgo užpilda <50%, jis suliejamas su kaimynais.
- Nustatyta, kad darbo metu po įterpimų ir trynimų B medžio užpilda yra apie 69%
- **Mazgo dydis**: (p*P)+((p-1)*(Pr+V)) ≤ B

### B<sup>+</sup> tree

- Idėja – rodykles į duomenis saugoti tik lapuose, todėl lapų ir vidinių mazgų struktūra yra skirtinga.
- **Vidiniai mazgai**  <P<sub>1</sub>, K<sub>1</sub>, ... , P<sub>q</sub>, K<sub>q</sub>>
- **Lapai** <P<sub>1</sub>, <K<sub>1</sub>, Pr<sub>1</sub>>, ... , P<sub>q</sub>, <K<sub>q</sub>, Pr<sub>q</sub>>>
- Kadangi vidiniuose mazguose nebėra rodyklių į duomenis, galim sukišt daugiau rodyklių į mazgus. Didesnis šakojimosi koeficientas – mažesnė medžio struktūra (mažiau lygmenų).

### Hash file

- Idėja – yra funkcija h, vadinama atsitiktine maišos f-ja, kuri veikia įrašo maišos lauko reikšmę ir atiduoda bloko, kuriame yra įrašas, adresą.
- Jei laukas K yra ne sveikas skaičius, tai bet kurį tipą galima tokiu padaryti.
- Paprasčiausia hashing funkcija - h(K) = K mod M
- Grėsmė - **collisions**. Sprendimai:
    - nuoseklus adresavimas **(open addressing)** – nuosekliai ieškomi visi sekantys adresai, kol bus rastas neužimtas hash.
    - plėtimas grandinėle **(chaining)** - adresų erdvė išplečiama, kiekvienam įrašui papildomai pridedama rodyklė. Įrašas rašomas į papildomą adresų erdvę
    , o f-jos adreso rodyklė rodo papildomą adresą, šio rodyklė – kitą papildomą adresą ir pan.
    - daugiapakopė maiša **(multiple hashing)** – esant kolizijai, pritaikoma kita f-ja, jei ir ji sudaro koliziją, ji sprendžiama nuoseklaus adresavimo būdu ar pritaikoma dar viena f-ja, jei ji sudaro koliziją ...
- **External hashing** - rodoma ne į įrašo, o į bloko ar sektoriaus adresą. Funkcija rakto reikšmę paverčia reliatyviu adresu bloke. Kolizijos iškyla rečiau.
- **Trūkumai:**
    - Neefektyvu dideliems duomenų kiekiams (hashingas užtrunka)
    - Užima daugiau vietos
    - Paieška pagal neraktinį lauką - tokia pat neefektyvi, kaip ir kitais atvejais.

<hr>

## [Objektiškai orientuotos ir objektinės DB](#objektiškai-orientuotos-ir-objektinės-db)

- **RDBMS** modelio **trūkumai**:
    - Neatvaizduojami realaus pasaulio objektai
    - Vienalytė struktūra
    - Nėra galimybių apibrėžti naujas operacijas
    - Nėra rekursinio apdorojimo
    - Nesuderinama su programavimo kalbomis

<br>

- **Object - oriented DB** - OOP
- **Object - relational DB** - hibridinės
- Lengviau aprašyti sudėtingas struktūras.
- Trūkumas - sudėtinga standartizuoti
- **ID** generuojamas sistemos, GUID
- **Klasės**, single ir multiple inheritance
- **Overloading**, polymorphism

### Permanentinės programavimo kalbos
- Kalbos, kuriomis apdorojami tarpiniai duomenys gali būti išsaugoti, o vėliau - panaudojami kitų programų.
- **Object-relational impedence mismatch** - jei bandome naudoti SQL vietoje OODB kalbos
- **Pointer (object) swizzling** - ID pavertimas adresais
- **Veikimo principas:**
    1. Naudojant OID, nustatomas reikalingas blokas, duomenys iš disko keliami į cache
    1. Objektai susiejami nuorodomis, kitaip paruošiami
    1. Programa pasiekia objektą ir jį atnaujina
    1. Objektas permanentiškai išsaugomas, atlaisvinama cache.
- **Realizacija:** (skirtingi būdai)
    - Kontroliniame taške visas programos cache išsaugomas diske. Šį tašką pasiekti gali tik viena programa.
    - Serializacija, konservacija. Kuriamos deep kopijos. Duplikavimas.
    - Išsaugojimas puslapiais:
        - *Allocation-based* - turi būti nurodyta objektą paversti permanentiniu
        - *Reachability-based* - objektas permanentinis, jei prieinamas iš root permanent objekto.
- **Ortogonalusis permanentiškumas:**
    1. Duomenų permanentiškumas nepriklauso nuo to, koks programos fragmentas jį naudoja
    1. Nepriklauso nuo duomenų tipo. Visi tipai gali būti permanentiniai
    1. Tranzityvumas - identifikavimas ir organizacija nepriklauso nuo objektų tipų.
- **Reikalavimai ODBVS:**
    1. Complex objects
    1. Object identity
    1. Encapsulation
    1. Types and Classes
    1. Class or Type Hierarchies
    1. Overriding, overloading and late binding
    1. Computational completeness
    1. Extensibility
    1. Persistence
    1. Secondary storage management
    1. Concurrency
    1. Recovery
    1. Ad Hoc Query Facility 

<hr>

## [Dedukcinės DB](#dedukcinės-db)

- DB valdymo sistemos, galinčios daryti logines išvadas iš jose laikomų faktų ir taisyklių
- **Faktai** - DB lentelių eilutės
- **Taisyklės** - analogiškos rodiniams, tik papildomai leidžia rekursines užklausas
- **Prolog:**
    - Prolog's single data type is the term. Terms are either atoms, numbers, variables or compound terms.
        - An atom is a general-purpose name with no inherent meaning. Examples of atoms include x, red, 'Taco', and 'some atom'.
        - Numbers can be floats or integers. ISO standard compatible Prolog systems can check the Prolog flag "bounded". Most of the major Prolog systems support arbitrary precision integer numbers.
        - Variables are denoted by a string consisting of letters, numbers and underscore characters, and beginning with an upper-case letter or underscore. Variables closely resemble variables in logic in that they are placeholders for arbitrary terms.
        - A compound term is composed of an atom called a "functor" and a number of "arguments", which are again terms. Compound terms are ordinarily written as a functor followed by a comma-separated list of argument terms, which is contained in parentheses. The number of arguments is called the term's arity. An atom can be regarded as a compound term with arity zero. Examples of compound terms are truck_year('Mazda', 1986) and 'Person_Friends'(zelda,[tom,jim]).
    - Prolog programs describe relations, defined by means of clauses. Pure Prolog is restricted to Horn clauses. There are two types of clauses: facts and rules. A rule is of the form
    - is tom a cat?
    <br>
    ```
    ?- cat(tom).
    Yes
    ```
    - what things are cats?
    <br>
    ```
    ?- cat(X).
    X = tom
    ```
- **Datalog**. In contrast to Prolog, Datalog
    - disallows complex terms as arguments of predicates, e.g., p (1, 2) is admissible but not p (f (1), 2),
    - imposes certain stratification restrictions on the use of negation and recursion,
    - requires that every variable that appears in the head of a clause also appears in a nonarithmetic positive (i.e. not negated) literal in the body of the clause,
    - requires that every variable appearing in a negative literal in the body of a clause also appears in some positive literal in the body of the clause[4]

<hr>

## [Aktyviosios DB](#aktyviosios-db)

- Apibrėžimas: **Aktyvioji DB sistema (ADBS)** - tai sistema, gebanti reaguoti į aplinkos įvykius.
- Aktyvios sistemos yra siejamos su taisyklės (sintaksinė išraiška, aprašanti sistemos reakciją) sąvoka. Aktyvios taisyklės dažnai siejamos su trigger’iais, aprašomos naudojant įvykis-sąlyga-veiksmas (ECA). 
- **ECA** - yra stebima ar įvyko įvykis, kuris aktyvuoja taisyklę. Taisyklės salyga, tai formulė, kuri turi būti tenkinama, kad būtų įvykdomas veiksmas. Veiksmas aprašo ką reikia daryti, kai taisyklė yra aktyvuojama ir jos sąlyga tenkinama.
- Aktyvios duomenų bazės yra pasyvios faktų saugyklos. Joje aktyviai veikia vartotojo apibrėžta komponentė, galinti įtakoti tiek DB, tiek išorinę aplinką. Tai daroma įvykių, valdomų ECA taisyklių pagalba.
- **Savybės**: 
    - ADBS yra tradicinė DBS, kuri yra praplėsta papildoma aktyvumo savybe, kuri leidžia automatiškai, savalaikiai ir efektyviai reaguoti į įvykius. 
    - ADBS gali atlikti operacijas automatiškai  atsižvelgiant į susiklosčiusią situaciją DB viduje ar išorėje.
    - ADBS įgalina reaktyvios elgsenos specifikavimą ir įgyvendinimą.
- **DBVS aktyvavimo būdai**:
    - Jei DBVS yra aktyvuojama dažniau, negu to reikia, yra veltui naudojami DBVS resursai.
    - Jei DBVS yra aktyvuojama rečiau, negu to reikia, svarbūs įvykiai gali būti visai neaptikti arba aptikti ne laiku.
- **Verslo taisyklių įgyvendinimas**:
    - Taisyklės struktūriniai ribojimai - įgyvendinami dalykinės srities koncepciniame modelyje.
    - Taisyklės dinaminiai ribojimai - gali būti tiesiogiai išreikšti įvykis-taisyklė-veiksmas(ECA) taisyklėmis.

### Įvykiai
- **Įvykis** yra atsitikimas, įvykęs tam tikru laiko momentu.
- Apibrėžiant įvykį yra nurodoma, kas turi būti stebima.
- Būdas, kuriuo galima aptikti įvykį, priklauso nuo įvykio
šaltinio. 
- **Galimi šaltiniai**:
    - duomenų keitimas – įvykio pasirodymo priežastis – tam
    tikrų duomenų keitimas (insert, update, delete operacijos);
    - elgsenos iškvietimas – įvykis pasirodo vykdant vartotojo
    apibrėžtas operacijas;
    - transakcija – įvykio pasirodymo priežastis – transakcijų
    komandos;
    - prieštaravimai – įvykis pasirodo kaip prieštaravimo
    rezultatas;
    - laikrodis – įvykis pasirodo tam tikru laiko momentu;
    - išorė – įvykio pasirodymą nulemia už DB ribų vykstantys
    pokyčiai 
- **Problemos**:
    - Apdorojant daug įrašu generuojama daug įvykių, po vieną kiekvienam įrašui 
    - Kai laukiama įvykių poros, vienas įvykis įvyksta kelis kartus, o kitas tik vieną, kiek operacijų tada reikia atlikti
- **Sąlygos** dažniausiai yra išreiškiamos kaip DB predikatai, tačiau gali būti supaprastintos, kad būtų padidintas įvertinimo efektyvumas. Sąlygos gali apimti ir išorinių funkcijų iškvietimą. Sąlygos reikalingos įvykiams(panašiai kaip trigeriai)
- **Veiksmai gali**: 
    - atnaujinti DB ar taisyklių rinkinio struktūrą, 
    - sukelti tam tikrą elgseną DB viduje ar išorėje,
    - informuoti vartotoją ar administratorių apie tam tikrą situaciją, • nutraukti transakciją. 
- Veiksmas gali būti vykdomas, kaip priedas prie (in addition to) operacijos, kuri sukėlė atitinkamą įvykį arba vietoj jos (instead of).

<br>

- **Sąveika tarp taisyklių**
<br>Kai gautame taisyklių rinkinyje yra keletas tinkamų, ir jos galbūt prieštarauja viena kitai – įvairios strategijos, kurią iš jų vykdyti (o gal kelias). Pasirinkti galima kaip ekspertinėse sistemose. 
- Galima: 
    - Vykdyti visus taisyklės atvejus nuosekliai. 
    - Vykdyti visus taisyklės atvejus lygiagrečiai. 
    - Vykdyti visus konkrečios taisyklės atvejus, prieš tai, kol nenagrinėjamos kitos taisyklės. 
    - Vykdyti vieną ar keletą taisyklės atvejų

<br>

- **Baigtinumo patikra** 
<br>Sistemos, kuriose yra apibrėžta daug aktyvių taisyklių, dažniausiai yra cikliškos. Dauguma ciklų yra nepavojingi, nes tiesiog apibūdina abipusę sąveiką tarp taisyklių. Detalios ciklų analizės metu galima nustatyti, ar ciklas yra nepavojingas. 
