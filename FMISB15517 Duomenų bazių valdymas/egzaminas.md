# FMISB15517 Duomenų bazių valdymas

1. [SQL užklausų apdorojimas](#SQL-užklausų-apdorojimas)
2. [Išskirstytos duomenų bazės](#Išskirstytos-duomenų-bazės)
3. [Transakcijos](#Transakcijos)
4. [Duomenų bazės fiziniame lygmenyje](#Duomenų-bazės-fiziniame-lygmenyje)
5. [Duomenų bazės įrašai](#Duomenų-bazės-įrašai)

## [SQL užklausų apdorojimas](#SQL-užklausų-apdorojimas)

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

## [Išskirstytos duomenų bazės](#Išskirstytos-duomenų-bazės)

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

## [Transakcijos](#Transakcijos)
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

## [Duomenų bazės fiziniame lygmenyje](#Duomenų-bazės-fiziniame-lygmenyje)

<hr>

## [Duomenų bazės įrašai](#Duomenų-bazės-įrašai)

<hr>