# Plan: pilot točnosti profila ukusa i sličnosti korisnika

## Kontekst
Društvena mreža za događaje ima profile osoba i organizatora. Glavna posebnost je **profil ukusa**: osoba složi listu „definira me” po domenama. Iz profila se računa **sličnost s drugima**: ukupno, po domeni i po osima „kako konzumiraš”. Kasnije se iz publike računaju i profili događaja i organizatora.

Točnost profila ključna je za cijeli projekt, a sve ostalo je zasad nebitno. Istraživanje (tri izvještaja u `reports/`) pokazalo je:
- Ukus ima empirijski utemeljene osi koje se protežu preko medija.
- Odabrani favoriti su valjan signal za povezivanje ljudi. Prikazani ukus predviđa prijateljstva.
- Nitko nije javno provjerio „% sličnosti osoba” prema doživljaju ljudi. To mora dokazati naš pilot.
- Vanjski API-ji su uglavnom zatvoreni. Hrvatske stavke imaju vrlo slabe metapodatke u otvorenim katalozima.

Cilj ovog plana: **pilot s 30 do 50 stvarnih ljudi** koji usporedi više metoda izračuna profila i sličnosti s onim kako ljudi doživljavaju sličnost, i pokaže koja metoda radi.

Izvori:
- `reports/Profil ukusa i sličnost korisnika.md`
- `reports/Profil ukusa iz odabranih lista.md`
- `reports/Katalozi i rječnik ukusa.md`
- bilješke u `research_notes/`

## Donesene odluke
| Tema | Odluka |
|---|---|
| Podaci | Jedna lista „definira me” po domeni. Bez povijesti, bez ostatka računa, bez upitnika u aplikaciji. |
| Domene | Glazba, filmovi, serije, knjige, podcasti, video i društvene igre, hobiji, sport, hrana, umjetnost |
| Izvor stavki | Liste se slažu u alatu pretragom kataloga. Službeni API-ji samo gdje još rade. |
| Katalozi | Trajno samo otvoreni izvori: Wikidata, MusicBrainz (CC0 jezgra), Discogs izvozi, TVmaze, Open Library izvozi, Podcast Index. Bez TMDB-a, BGG-a, IMDb-a i Last.fm-a za komercijalnu upotrebu. |
| Praznine u metapodacima | Generirani opisi i žanrovi, označeni kao „generirano” |
| Osi | Psihološke osi (Estetsko, Mračno, Zajedničarsko, Uzbudljivo, Cerebralno + energija, raspoloženje, dubina) + „kako konzumiraš” (nišnost, staro/novo, širina). Pilot ih provjerava. |
| Sličnost | Unutar domena i preko domena. Domene se spajaju ponderirano po pouzdanosti (duljina i opisanost lista). |
| Metode | Isprobati više metoda i usporediti ih (vidi dolje) |
| Pilot | 30–50 poznanika. Liste slažu u jednostavnom alatu. Ocjenjuju anonimne liste 10–20 drugih. |
| Za kasnije | Profil događaja i organizatora, demografija, osjetljivi sadržaj, oblik prikaza postotka, slobodan unos pojmova, os aktivnosti (testira se u pilotu) |

## Faze

### Faza 1: Katalog i rječnik
1. Za svaku domenu definirati izvor stavki i polja koja trebaju za profil: naziv, vrsta, godina, žanrovi/tagovi, opis, poveznice na Wikidatu.
2. Za nemedijske domene složiti početni kontrolirani rječnik iz Wikidate (domena → kategorija → podkategorija → konkretna stavka) s hrvatskim nazivima i pretragom bez dijakritika. Za umjetnost Getty i Europeana, za sport OSM oznake.
3. Pretraga mora filtrirati po domeni. Kratki nazivi poput Murina, Kotlovina i Zaa vraćali su krive pogotke.
4. **Obogaćivanje:** za stavke bez opisa i žanrova generirati opis, žanrove i bodove na osima. Svaka generirana vrijednost ima oznaku i izvor. Na uzorku od oko 50 stavki ručno provjeriti točnost generiranja.

### Faza 2: Alat za liste (samo za pilot)
Jednostavna stranica na hrvatskom:
- privola sudionika (uključuje mogućnost da ukus otkrije osjetljive podatke, po presudi C-184/20)
- osnovni podaci: dob i spol, samo za kasniju analizu demografije
- po domeni: pretraga, odabir oko 10 stavki, poredak i neobavezna kratka napomena
- korak ocjenjivanja: sudionik vidi anonimne liste 10–20 drugih i za svaku ocijeni „Koliko je ukus ove osobe sličan tvome?” (ukupno i po domeni)
- **ponovni unos nakon 3–4 tjedna:** sudionik iznova složi liste, kako bismo izmjerili stabilnost

Tehnologija se bira na početku ove faze.

### Faza 3: Profili (više varijanti)
Za svaku stavku izračunati dvije reprezentacije:
- **semantički vektor** iz opisa i metapodataka. Usporediti nekoliko modela i nekoliko načina sastavljanja opisa.
- **bodovi na osima:** projekcija na sidrene opise krajeva osi, ili procjena jezičnim modelom. Usporediti obje.

Profil osobe po domeni je skup ponderiranih stavki (poredak i manja težina popularnih stavki). Osi „kako konzumiraš” računaju se iz liste: nišnost, prosječna starost stavki, širina.

### Faza 4: Metode sličnosti (eksperiment)
Svaka metoda računa sličnost za sve parove, po domeni i ukupno:
1. **Prosječni vektor + kosinus**
2. **Uparivanje stavki:** optimalni transport ili simetrično najbolje uparivanje
3. **Udaljenost na osima ukusa**
4. **Izravno preklapanje s IDF-om:** samostalno i kao bonus
5. **Kombinacije** gornjih metoda
6. **Kontrole:** nasumična sličnost i sličnost samo po dobi i spolu

Spajanje domena u ukupnu sličnost:
- ponderirano po pouzdanosti, uspoređeno s jednakim težinama
- ukupni profil preko svih domena, uspoređeno s prosjekom sličnosti po domenama

Dodatne varijante za usporedbu:
- s osi aktivnosti i bez nje
- s generiranim opisima i bez njih

### Faza 5: Vrednovanje
- **Glavna metrika:** korelacija (Spearman) između izračunate i procijenjene sličnosti, po metodi i po domeni. Prag uspjeha je oko r ≥ 0,30. Uz to rangiranje: jesu li osobe koje je sudionik ocijenio najsličnijima i kod metode pri vrhu.
- **Predviđanje skrivenih stavki:** sakrije se jedna domena (npr. knjige) i provjeri predviđaju li ostale domene koje su osobe po knjigama slične. To je test preko domena.
- **Provjera osi:** faktorska analiza bodova iz pilota. Potvrđuje li se struktura osi?
- **Stabilnost:** korelacija profila i sličnosti između prvog i ponovljenog unosa.
- **Pravednost:** točnost posebno za osobe s pretežno hrvatskim i pretežno stranim stavkama, te s generiranim opisima i bez njih.
- **Demografija:** koliki dio sličnosti objašnjavaju dob i spol (podloga za kasniju odluku).
- **Objašnjivost:** za najbolje metode prikazati 2–3 razloga sličnosti, a sudionici na manjem uzorku ocijene imaju li smisla.

### Faza 6: Zaključak pilota
Izvještaj s rezultatima: koja metoda i koje težine pobjeđuju, koliko je postotak pouzdan, gdje profil griješi, koliko stavki treba. Na temelju toga:
- odluke o prikazu postotka, demografiji i osjetljivom sadržaju
- drugi krug pilota s 85 ili više sudionika, potreban za pouzdan zaključak
- zatim profil događaja i organizatora

## Provjera da plan radi
- **Faza 1:** svaka domena ima pretragu koja za uzorak hrvatskih i stranih stavki vraća točan pogodak. Generirani opisi prolaze ručnu provjeru na uzorku.
- **Faza 2:** pilot sudionik može bez pomoći složiti liste i ocijeniti druge za manje od 20 minuta. Testirati na 2–3 osobe prije cijele skupine.
- **Faze 3–5:** prije pravih podataka cijeli tijek provjeriti na nekoliko izmišljenih persona. To je samo tehnička provjera, ne dokaz točnosti, jer bi zaključak bio kružan. Zatim pokrenuti na podacima pilota i proizvesti tablicu metoda × metrika.
