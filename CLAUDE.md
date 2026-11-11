# CLAUDE.md

## Projekt
Društvena mreža za događaje (sučelje na hrvatskom, tržište Hrvatska). Dvije vrste profila: **osoba** i **organizator događaja**. Organizatori objavljuju događaje (datum, lokacija, tagovi, opis), a osobe označavaju da dolaze.

Glavna posebnost je **profil ukusa**. Osoba složi listu „definira me” po domenama, a iz profila se računa **sličnost s drugim korisnicima**: ukupno, po domeni i po osima „kako konzumiraš”. Kasnije se iz publike grade i profili događaja i organizatora te preporuke.

**Točnost profila i način na koji ga dobivamo ključni su aspekt projekta. Sve ostalo je zasad nebitno.**

## Trenutno stanje
- Istraživanje je gotovo. Pogledaj `reports/` (tri izvještaja) i `research_notes/` (bilješke s izvorima).
- Odobreni plan je u `PLAN.md`: pilot s 30–50 stvarnih ljudi koji uspoređuje više metoda sličnosti s ocjenama sudionika.
- **Opseg je sužen na glazbu i filmove.** Ostale domene dodajemo tek ako pilot pokaže da pristup radi.
- Sljedeći korak: **Faza 1, specifikacija kataloga** za glazbu i film. Prvo dokument koji pregledavamo zajedno, ne kod.

## Način rada (važno)
- Komuniciraj na **hrvatskom**.
- **Planiraj dugo i temeljito prije implementacije.** Ključne odluke podupri istraživanjem (literatura, slični proizvodi, izvori podataka). Pitanja postavljaj u krugovima.
- Primjere koje korisnik navede (npr. „kosinusna sličnost”, „Spotify/IMDb”) shvati kao ilustraciju, ne kao konačnu odluku. Sam predlaži više opcija.
- **Ne spominji tehnološki stack** (jezike, frameworkove, baze) dok korisnik ne kaže da je vrijeme. Plan kaže da se tehnologija bira na početku Faze 2.

## Donesene odluke
- **Podaci:** jedna lista „definira me” po domeni. Ne čita se povijest ni ostatak korisničkog računa. U aplikaciji nema upitnika.
- **Domene:** za početak samo glazba i filmovi. Kasnije, ako pilot uspije: serije, knjige, podcasti, video i društvene igre, hobiji, sport, hrana, umjetnost.
- **Izvor stavki:** liste se slažu u aplikaciji pretragom kataloga. Službeni API-ji koriste se samo gdje još rade (npr. YouTube; Spotify od 2026. ne daje sadržaj tuđih playlista).
- **Katalozi:** trajno samo otvoreni izvori. Glazba: MusicBrainz CC0 jezgra, Discogs izvozi, Wikidata. Film: Wikidata. Za kasnije domene: TVmaze, Open Library izvozi, Podcast Index. Bez TMDB-a, BGG-a, IMDb-a i Last.fm-a za komercijalnu upotrebu.
- **Hrvatske stavke s oskudnim metapodacima:** dobivaju generirane opise i žanrove, označene kao „generirano”.
- **Osi ukusa:** psihološke osi (Estetsko, Mračno, Zajedničarsko, Uzbudljivo, Cerebralno + energija, raspoloženje, dubina) i osi „kako konzumiraš” (nišnost, staro/novo, širina). Pilot ih provjerava.
- **Sličnost:** računa se unutar i preko domena. Domene se spajaju ponderirano po pouzdanosti. Pilot uspoređuje više metoda: prosječni vektor, uparivanje stavki ili optimalni transport, udaljenost na osima, preklapanje s IDF-om te njihove kombinacije.
- **Pilot:** 30–50 poznanika. Liste slažu u jednostavnom alatu i ocjenjuju anonimne liste 10–20 drugih. Profil događaja nije dio pilota.
- **Odgođeno:** profil događaja i organizatora, uloga demografije, osjetljivi sadržaj (GDPR čl. 9, presuda C-184/20), oblik prikaza postotka, slobodan unos pojmova, os aktivnosti (testira se u pilotu).
