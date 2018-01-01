# FMISB15517 Duomenų bazių valdymas
## SQL užklausų apdorojimas
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

 7. 
 8. 
 9. 
10. 
11. 
12. 

### Siūlomi optimizacijos algoritmo žingsniai

<hr>

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

## Išskirstytos duomenų bazės

<hr>

## Transakcijos

<hr>

## Duomenų bazės fiziniame lygmenyje

<hr>

## Duomenų bazės įrašai

<hr>