# Faza 1: specifikacija kataloga za glazbu i film

**Status:** nacrt za zajednički pregled. Na kraju su otvorena pitanja za prvi krug odluka. Tehnologija se ovdje namjerno ne spominje, jer se bira na početku Faze 2.

## Svrha

Katalog je sve što sudionik pilota može staviti na listu „definira me”, zajedno s podacima iz kojih računamo profil. Ovaj dokument određuje:
1. koje vrste stavki se mogu birati
2. iz kojih izvora dolaze i koja polja nam trebaju
3. kako se izvori povezuju
4. kako radi pretraga
5. gdje i kako popunjavamo praznine u opisima
6. kako provjeravamo da katalog radi prije pilota

Točnost profila ovisi o tome ima li svaka stavka dovoljno opisa da se usporedi s drugima. Dosadašnje istraživanje pokazalo je da hrvatske stavke postoje u katalozima, ali su slabo opisane. Ovaj dokument zato najviše prostora daje sloju opisa.

## Novi nalazi ovog kruga

Prije pisanja provjerio sam tri stvari koje su u istraživanju ostale otvorene. Detalji i sirovi brojevi su u `research_notes/Katalozi i rječnik ukusa/mjerenja_glazba_film.md`.

1. **MusicBrainz žanrovi nisu otvoreni.** Dodjela žanrova i oznaka izvođačima spada u „dopunske podatke” pod licencom CC BY-NC-SA, dakle samo za nekomercijalnu uporabu. CC0 su identitet, veze i sam popis žanrova kao rječnik.
2. **Discogs pokriva hrvatsku glazbu puno bolje od očekivanog.** Svih 18 hrvatskih i regionalnih izvođača s Discogs ID-jem ima žanrove i stilove, uključujući hip-hop i elektroniku, gdje MusicBrainz nema ništa. Stilovi pripadaju izdanjima, pa nose šum iz kompilacija i suradnji.
3. **Hrvatski film na Wikidati ima identitet, ali ne i opis.** Za 394 filma IMDb ID ima 92%, a redatelja 84%. Žanr je najčešće samo jedan, i to „drama”. Opis na hrvatskom ima 12% filmova, a članak na hrvatskoj Wikipediji 52%.

Posljedica: **za glazbu je Discogs glavni izvor pokrivenosti žanrova. Za film otvoreni izvori nisu dovoljni za opis.**

## Odluke iz prvog kruga

| Tema | Odluka |
|---|---|
| Komercijalnost | Mreža je nekomercijalna. Izvori s nekomercijalnom licencom su dopušteni. Ako mreža jednom postane komercijalna, licence se tada rješavaju iznova. |
| MusicBrainz žanrovi | Koriste se (CC BY-NC-SA), uz navod izvora |
| Last.fm | Koristi se za oznake izvođača. Pokrivenost hrvatskih izvođača treba izmjeriti (potreban API ključ). |
| TMDB | Koristi se za film, na odgovornost korisnika, unatoč klauzuli koja zabranjuje korištenje za treniranje i validaciju sustava strojnog učenja |
| IMDb, Spotify | Zasad se ne koriste kao izvor podataka, samo kao poveznice |

Uvjeti koje treba poštovati:
- **Navod izvora** za MusicBrainz, Last.fm i TMDB. TMDB traži svoj logo i rečenicu da proizvod koristi TMDB API, ali ga TMDB ne podržava.
- **ShareAlike za MusicBrainz:** izvedeni podaci o žanrovima moraju biti dostupni pod istom licencom. To se odnosi na podatke o stavkama, ne na profile korisnika.
- **Rok čuvanja podataka.** TMDB ograničava keširanje, a prethodni izvještaj navodi najviše šest mjeseci. Podatke treba povremeno osvježavati, pa se TMDB u zapisu stavke drži odvojeno od ostalih izvora **[točan uvjet za nekomercijalnu uporabu treba ponovno pročitati]**. Za Last.fm uvjete čuvanja tek treba provjeriti.

## Odluke iz drugog kruga

| Tema | Odluka |
|---|---|
| Stavke u glazbi | **Pjesme.** Lista glazbe treba moći postati playlista. |
| Stavke u filmu | **Filmovi.** Redatelji, glumci i ostalo izvlače se iz filma kao metapodaci, a ne biraju se zasebno. |
| Ručni unos | Zasad dopušten. Takve stavke se ručno povezuju ili opisuju prije analize. |
| Prednost domaćim stavkama u pretrazi | Ne |
| Lokalni žanrovi | Ne dodaju se. Rječnik žanrova gradi se samo iz izvora. |

## Načela koja vrijede za obje domene

Ova načela proizlaze iz već donesenih odluka i izvještaja „Katalozi i rječnik ukusa”.

- **Kanonski identitet stavke** je Wikidata QID. Ako ga nema, stavka dobiva vlastiti ID, uz MBID ili Discogs ID kao poveznicu. ID vlasničkog servisa (Spotify, IMDb, TMDB) nikad nije kanonski. Smije se spremiti samo kao poveznica.
- **Izvori:** Wikidata, MusicBrainz, Discogs dumpovi, Last.fm i TMDB, prema odlukama iz prvog kruga. Tekst Wikipedije (CC BY-SA) koristi se uz uvjet koji je otvoreno pitanje 6.
- **Svaki podatak nosi izvor.** Uz svaki žanr, opis i bod na osi bilježi se odakle dolazi: Discogs, Wikidata, Wikipedija, generirano ili uredničko.
- **Generirani sadržaj je uvijek označen** kao „generirano” i odvojen od kataloških podataka. Tako ga možemo uključiti ili isključiti pri usporedbi metoda (plan, Faza 4).
- **Čuvaju se svi nazivi:** hrvatski, izvorni, engleski i aliasi. Katalozi kao prikazni naziv često vraćaju engleski.

## Glazba

### Što se može birati

| Vrsta stavke | Za | Protiv |
|---|---|---|
| **Izvođač** (osoba ili sastav) | Ljudi tako razmišljaju o glazbenom ukusu. Najbolja pokrivenost u svim izvorima. Preklapanje među ljudima je često. | Izvođač s dugom karijerom pokriva više stilova. |
| **Album** | Precizniji signal (rani i kasni Radiohead). Discogs stilovi su vezani uz izdanja. | Manje preklapanja. Slabija pokrivenost novih digitalnih izdanja. |
| **Pjesma** | Najbliže onome što ljudi slušaju. | Najmanje preklapanja. Najslabiji otvoreni metapodaci. Jedna pjesma malo govori o ukusu. |
| **Žanr ili scena** | Hvata ukus kad osoba ne može nabrojiti izvođače („tamburica”, „techno”). | Slab razlikovni signal. Uvodi rječnik žanrova u sučelje. |

**Odluka:** lista se sastoji od **pjesama**, jer lista treba moći postati playlista. Izvođači i albumi ne biraju se zasebno, ali svaka pjesma nosi podatke o svom izvođaču i albumu.

### Posljedice odabira pjesama

Pjesma je razina na kojoj su otvoreni metapodaci najslabiji. Provjera na uzorku od 11 poznatih pjesama (`research_notes/…/mjerenja_glazba_film.md`, odjeljak 4) pokazala je:
- **Snimke na MusicBrainzu nemaju žanrove ni oznake**, čak ni Creep, bad guy ili Do I Wanna Know?. Žanrovi na MusicBrainzu žive na razini izvođača i albuma.
- **Ista pjesma postoji u mnogo snimki.** „Creep” ima 194 pogotka (studijska, uživo, remaster, obrade), a „Cesarica” 15. Za preklapanje među ljudima treba odrediti što je „ista pjesma”.
- Discogs stilovi vezani su uz izdanje, pa pjesma može naslijediti stilove albuma ili singla na kojem je izašla.

Iz toga slijede tri pravila:
1. **Profil pjesme je slojevit.** Pjesma nasljeđuje opis albuma i izvođača, a vlastiti opis dobiva samo gdje on postoji (vidi „Kako se opisuje pjesma”).
2. **Kanonska pjesma** je skup svih snimki iste skladbe istog izvođača. Studijska, remaster i uživo verzije su ista pjesma. Obrada drugog izvođača i remiks su zasebne pjesme. Na MusicBrainzu to odgovara „djelu” (work) uz izvođača, ili grupiranju snimki po naslovu, izvođaču i ISRC-u. Pravilo treba provjeriti na uzorku.
3. **Preklapanje se računa na više razina:** ista pjesma, isti album, isti izvođač, isti žanr. Dvije osobe koje vole različite pjesme Arctic Monkeysa ipak dijele ukus. To ulazi u metode sličnosti u Fazi 4 kao „preklapanje s IDF-om po razinama”.

### Kako se opisuje pjesma

| Sloj | Izvor | Pokrivenost |
|---|---|---|
| Izvođač: žanrovi, država, razdoblje | Discogs, MusicBrainz, Last.fm, Wikidata (kao za izvođače gore) | dobra |
| Album ili singl: stilovi, godina | Discogs (stilovi izdanja), MusicBrainz (release group) | dobra za Discogs |
| Pjesma: oznake | **Last.fm** (top tags pjesme) | vjerojatno dobra za popularne pjesme, slaba za nišne **[nije izmjereno, treba API ključ]** |
| Pjesma: zvučna obilježja (tempo, energija, raspoloženje, akustičnost) | vidi dolje | otvoreno |
| Pjesma: generirani opis i bodovi na osima | generirano iz gornjih slojeva | sve stavke, označeno |

**Zvučna obilježja** bila bi najvrjedniji vlastiti signal pjesme, jer razlikuju laganu i energičnu pjesmu istog izvođača. Mogućnosti:
- **Spotify audio features:** zatvoreno za nove aplikacije od 27. 11. 2024. ([Spotify](https://developer.spotify.com/blog/2024-11-27-changes-to-the-web-api)). Otpada.
- **AcousticBrainz:** otvoreni skup obilježja za oko 29 milijuna snimki, prikupljanje zaustavljeno 2022. ([AcousticBrainz download](https://acousticbrainz.org/download)). Je li još dostupan i koliko pokriva hrvatske pjesme, nije provjereno. Novije pjesme sigurno nema.
- **Vlastita analiza zvuka** iz javnih isječaka od 30 sekundi (npr. Deezer). Pokriva sve pjesme, ali je veći posao, a uvjete korištenja isječaka treba provjeriti.
- **Bez zvučnih obilježja:** energiju i raspoloženje procjenjuje generator iz oznaka i naziva. Najjednostavnije, ali najmanje pouzdano za nišne pjesme.

To je otvoreno pitanje 13.

### Izvori i polja

| Polje | Izvor | Licenca | Napomena |
|---|---|---|---|
| Pjesma: identitet, naslov, trajanje, ISRC | MusicBrainz (snimka i djelo) | CC0 | Wikidata ima stavke samo za poznate pjesme, pa je za pjesme kanonski MusicBrainz |
| Pjesma: album ili singl, godina izlaska | MusicBrainz, Discogs | CC0 | |
| Pjesma: oznake | Last.fm (top tags pjesme) | nekomercijalno uz navod | |
| Pjesma: poveznice za slušanje | Spotify, YouTube, Deezer ID | samo ID | Za playlistu. Preko MusicBrainz URL veza, ISRC-a ili pretrage. |
| Izvođač: identitet, nazivi, aliasi | Wikidata, MusicBrainz | CC0 | MusicBrainz ima više aliasa i pravopisnih varijanti |
| Vrsta (osoba ili sastav), država, godine aktivnosti | MusicBrainz, Wikidata | CC0 | Kod 2 od 22 izvođača iz uzorka država nije upisana na MusicBrainzu |
| Žanrovi i stilovi | **Discogs dumpovi** (stilovi izdanja) | CC0 | Najbolja pokrivenost hrvatskih izvođača (18 od 18). Agregiraju se na razinu izvođača (vidi dolje). |
| Žanrovi i oznake | **MusicBrainz** | CC BY-NC-SA | Vezani uz izvođača i manje šumni, ali ih ima samo 7 od 22 hrvatskih izvođača |
| Oznake | **Last.fm** (top tags izvođača) | nekomercijalno uz navod | Bogate i opisne („melancholic”, „croatian”). Pokrivenost hrvatskih izvođača nije izmjerena. |
| Žanr | Wikidata (P136) | CC0 | Dopuna. Obično samo jedan, grub. |
| Diskografija s godinama | MusicBrainz (release group), Discogs (master) | CC0 | Za os „staro/novo” i za ponderiranje stilova |
| Opis | hr ili en Wikipedija, ili generirano | CC BY-SA ili vlastito | Hrvatski opis na Wikidati ima 12 od 20 izvođača |
| Popularnost | Broj Wikipedija (sitelinks) na Wikidati; Discogs kolekcije | CC0; za Discogs neprovjereno | Za os nišnosti i IDF težine |
| Poveznice | Wikidata (P434 MBID, P1953 Discogs, P1902 Spotify, P2722 Deezer) | CC0 | Samo za povezivanje i prikaz, nikad kao izvor opisa |

### Kako se iz izdanja dobiju žanrovi izvođača

Discogs stilovi pripadaju izdanjima. Unija stilova s tri izdanja dala je Haustoru „Hip Hop” i Krankšvesteru „Schlager”, vjerojatno iz kompilacija. Predlažem:
1. uzeti samo izdanja na kojima je izvođač glavni (ne kompilacije raznih izvođača, ne gostovanja)
2. preferirati albume (master) nad singlovima i reizdanjima
3. stil ulazi u profil izvođača ako se pojavljuje na barem određenom udjelu izdanja (početni prijedlog: 25%), s težinom prema tom udjelu
4. dodatno ponderirati izdanjima koja ljudi više imaju, ako se podatak o kolekcijama smije koristiti

Prag i težine provjeravaju se ručno na uzorku izvođača (vidi „Provjera”).

### Kako se spajaju izvori žanrova

Četiri izvora imaju različite prednosti:
- MusicBrainz je točan, ali rijedak.
- Discogs pokriva gotovo sve, ali nosi šum iz izdanja.
- Last.fm ima bogate i opisne oznake, ali one miješaju žanr, raspoloženje i nebitne oznake („seen live”).
- Wikidata daje jedan grubi žanr.

Prijedlog spajanja:
1. Svaki izvor preslika se na zajednički rječnik žanrova (vidi dolje). Last.fm oznake koje nisu žanr (raspoloženje, epoha, jezik) idu u obilježja ili se odbacuju.
2. Žanr koji potvrđuje više izvora dobiva veću težinu.
3. Ako se izvori izrazito ne slažu (Krankšvester je na MusicBrainzu „gabber”, a na Wikidati hip-hop), stavka ide na ručnu provjeru.
4. Bilježi se koliko izvora stoji iza svakog žanra. To je ulaz za ponder pouzdanosti u Fazi 4.

Varijanta za Fazu 4: pilot može usporediti profile iz svih izvora s profilima samo iz otvorenih (CC0) izvora. Tako saznajemo koliko točnosti donose nekomercijalni izvori, što je važno ako mreža jednom postane komercijalna.

### Rječnik žanrova

Za sličnost trebamo jedan zajednički rječnik u koji se preslikavaju svi izvori. Discogs ima oko 15 žanrova i nekoliko stotina stilova **[broj neprovjeren]**. Wikidata i MusicBrainz imaju vlastite popise.

**Prijedlog:**
- Osnovica su Discogs stilovi, jer su najpotpuniji za hrvatske stavke.
- Uz njih ide preslikavanje Wikidatinih i MusicBrainz žanrova te Last.fm oznaka na Discogs stilove.
- Lokalni žanrovi se ne dodaju (odluka iz drugog kruga). Ono što izvori imaju, na primjer Last.fm oznake poput „klapa”, ulazi preko preslikavanja.

## Film

### Što se može birati

| Vrsta stavke | Za | Protiv |
|---|---|---|
| **Film** | Prirodna jedinica. Dobar identitet na Wikidati. | Hrvatski filmovi imaju slab opis. |
| **Redatelj** | Jak signal autorskog ukusa („volim Lanthimosa”). | Rijeđi način razmišljanja kod većine ljudi. Miješa se s filmovima na istoj listi. |
| **Serijal ili franšiza** (npr. Gospodar prstenova) | Ljudi tako i misle o tome. | Nejasno je je li to jedna stavka ili više njih. |

**Odluka:** lista se sastoji od **filmova**. Redatelji, glumci i ostali podaci izvlače se iz filma kao metapodaci. Serije ostaju izvan opsega. Prijedlog za vrste filmova: dokumentarni i animirani su dopušteni, kratki nisu (otvoreno pitanje 2).

### Izvori i polja

| Polje | Izvor | Licenca | Napomena |
|---|---|---|---|
| Identitet, naslovi (hr, izvorni, en), aliasi | Wikidata | CC0 | Hrvatski naslov ima 61% hrvatskih filmova |
| Godina, trajanje, država, izvorni jezik | Wikidata (P577, P2047, P495, P364) | CC0 | |
| Redatelj, scenarist, glavni glumci, skladatelj | Wikidata (P57, P58, P161, P86) | CC0 | Redatelja ima 84% hrvatskih filmova. Ljudi su ujedno signal sličnosti. |
| Žanr | Wikidata (P136), TMDB | CC0; TMDB uvjeti | Wikidata je pregruba (kod hrvatskih filmova najčešće samo „drama”). TMDB ima oko 19 žanrova i dodjeljuje ih više po filmu. |
| Ključne riječi | **TMDB** (keywords) | TMDB uvjeti | Teme i motivi („dysfunctional family”, „island”). Mogu biti osnova sloja obilježja. Pokrivenost hrvatskih filmova nije izmjerena. |
| Sažetak radnje | **TMDB** (overview, i na hrvatskom gdje postoji) | TMDB uvjeti | Dopuna Wikipediji, posebno kad članka nema. Hrvatski sažeci nisu izmjereni. |
| Alternativni naslovi | TMDB, Wikidata | – | Za pretragu |
| Glavna tema, radnja smještena u | Wikidata (P921, P840) | CC0 | Kod stranih filmova češće, kod hrvatskih rijetko **[nije izmjereno]** |
| Opis i radnja | hr ili en Wikipedija | CC BY-SA | Glavni izvor teksta. Hrvatski članak ima 52% hrvatskih filmova. |
| Generirani opis, žanrovi i bodovi na osima | generirano iz gornjeg teksta | vlastito | Za sve stavke, ali označeno |
| Popularnost | Broj Wikipedija (sitelinks); TMDB popularnost i broj glasova | CC0; TMDB uvjeti | Za hrvatske filmove 6–19 Wikipedija, za poznate strane 80+ |
| Poveznice | TMDB ID (P4947) za dohvat; IMDb (P345) samo kao poveznica | – | TMDB ID ima većina hrvatskih filmova na Wikidati |
| Plakat | – | – | Otvoreno pitanje 8 |

### Rječnik žanrova i obilježja

Wikidatini žanrovi ne razlikuju dovoljno. „Drama” opisuje i Zvizdan i Fine mrtve djevojke. Zato predlažem dva sloja:
1. **Žanrovi:** mali kontrolirani popis od oko 25–30 žanrova (drama, komedija, triler, horor, dokumentarni, animirani…), na koji se preslikavaju Wikidatini žanrovi.
2. **Obilježja:** opisni pojmovi koji razlikuju ukus unutar žanra, na primjer „spori ritam”, „društvena kritika”, „crni humor”, „poslijeratna tema”, „autorski”, „žanrovski”, „vizualno stiliziran”. Izvori su TMDB ključne riječi, preslikane na kontrolirani popis, i generiranje iz teksta. Pregledavaju se na uzorku.

Obilježja su ujedno most prema psihološkim osima (Mračno, Cerebralno, Estetsko…). Studija Chamorro-Premuzica (2010) pokazala je da laičke, perceptivne kategorije bolje prate osobnost od stručnih. To podupire ovaj sloj.

### Hrvatski i regionalni film

Na Wikidati je 394 hrvatska filma i oko 1.400 jugoslavenskih (stavke tipa „film”, bez podtipova). Jugoslavenski filmovi uglavnom imaju oznaku države „Jugoslavija” (Q36704), a ne „SFRJ”. Zato:
- Oznaka „hrvatsko ili regionalno” obuhvaća Hrvatsku, Jugoslaviju, SFRJ, BiH, Srbiju, Sloveniju, Crnu Goru i Sjevernu Makedoniju. Služi za mjerenje pravednosti u Fazi 5, ne za filtriranje.
- Koliko filmova nedostaje, može se provjeriti usporedbom s HAVC-ovim katalogom od 2008. To je posao za kasnije, osim ako pilot pokaže rupe.

## Povezivanje izvora

Wikidata je čvorište. Jedna stavka sastavlja se ovako:

```
Wikidata QID
 ├─ MusicBrainz MBID (P434) → aliasi, vrsta, država, diskografija
 ├─ Discogs ID (P1953 izvođač, P1954 master) → žanrovi i stilovi izdanja
 ├─ Last.fm (preko naziva ili MBID-a) → oznake izvođača
 ├─ TMDB ID (P4947) → žanrovi, ključne riječi, sažetak, alternativni naslovi
 ├─ hr/en Wikipedija (sitelinks) → tekst opisa, broj Wikipedija kao popularnost
 └─ IMDb, Spotify, Deezer ID → samo poveznice
```

Za pjesme je čvorište MusicBrainz, a ne Wikidata, jer Wikidata ima stavke samo za poznate pjesme. Pjesma se veže na snimku i djelo na MusicBrainzu, a preko izvođača i albuma na Wikidatu i Discogs.

Stavka bez Wikidata QID-a (novi izvođač, mali film) povezuje se preko MBID-a ili Discogs ID-a i dobiva vlastiti ID. Prema prvom mjerenju, MusicBrainz ima vezu na Wikidatu kod 16 od 22, a Wikidata ima Discogs ID kod 19 od 20 hrvatskih izvođača.

### Zapis stavke

Opis sadržaja, bez odluke o tehnologiji:

| Grupa | Polja |
|---|---|
| Identitet | vlastiti ID, Wikidata QID, MBID, Discogs ID, domena, vrsta (pjesma, film) |
| Veze (pjesme) | izvođač, album ili singl, sve snimke koje čine istu pjesmu, poveznice za slušanje |
| Nazivi | hrvatski, izvorni, engleski, aliasi, verzija bez dijakritika za pretragu |
| Činjenice | godina ili razdoblje, država, jezik, ljudi (redatelj, članovi sastava) |
| Žanrovi i obilježja | popis vrijednosti iz rječnika, svaka s težinom i izvorom |
| Opis | tekst, jezik, izvor, oznaka „generirano” |
| Bodovi na osima | vrijednost po osi, način izračuna, oznaka „generirano” |
| Popularnost | broj Wikipedija, ostali signali ako su dopušteni |
| Porijeklo | oznaka hrvatsko, regionalno ili strano (za mjerenje pravednosti) |
| Kvaliteta | koliko je stavka opisana (za ponderiranje pouzdanosti u Fazi 4) |

## Pretraga

Prvi krug istraživanja pokazao je da je pretraga veći problem od pokrivenosti. „Murina” je vratila rod šišmiša, „Kotlovina” jelo, „Zaa” tri različita izvođača, a „Severina” duplikat s četiri obožavatelja.

Zahtjevi:
1. **Filtriranje po domeni i vrsti.** U glazbi se traže pjesme, a u filmu filmovi.
2. **Pretraga pjesme po naslovu i izvođaču.** Sudionik može upisati „cesarica”, „oliver cesarica” ili „cesarica dragojević”.
3. **Jedna pjesma, jedan rezultat.** Sve snimke iste pjesme (remasteri, reizdanja, kompilacije) prikazuju se kao jedan rezultat. Uživo i akustične verzije ne prikazuju se zasebno, osim ako to odlučimo.
4. **Bez dijakritika i s aliasima.** „sajeta” nalazi Šajetu, „kranksvester” Krankšvester, a „The High Sun” i „Zvizdan” isti film.
5. **Rangiranje** po popularnosti, da duplikati i prazne stavke padnu niže. Hrvatske stavke nemaju prednost (odluka iz drugog kruga).
6. **Razlikovni podaci u rezultatu.** Na primjer „Cesarica · Oliver Dragojević · 1993” ili „Murina · film · 2021 · Antoneta Alamat Kusijanović”.
7. **Kad stavke nema.** Sudionik upiše naslov (i izvođača) ručno. Stavka se označi i prije analize ručno poveže ili opiše (odluka iz drugog kruga).

Pretraga radi nad vlastitom kopijom kataloga, a ne uživo prema tuđim sučeljima. Razlozi su ograničenja broja upita (Wikidata je u ovom krugu vraćala HTTP 429 i 504), uvjeti korištenja i pravo baze podataka. Za pilot je dovoljan podskup kataloga (otvoreno pitanje 7).

## Obogaćivanje opisa

Generirani sadržaj popunjava praznine, a ne zamjenjuje katalog. Za svaku stavku:
1. **Ulaz:** svi otvoreni podaci o stavci (žanrovi, stilovi, ljudi, godina, država) i tekst s Wikipedije ako postoji.
2. **Izlaz:** kratak neutralan opis (2–3 rečenice), obilježja iz kontroliranog rječnika i bodovi na osima.
3. **Oznake:** svaka vrijednost nosi oznaku „generirano” i popis ulaza iz kojih je nastala.
4. **Kad nema dovoljno ulaza:** ako o stavci postoje samo naziv i godina, generator ne smije izmišljati. Stavka se označi kao „neopisana” i ide na uredničku dopunu.

Zadnja točka je važna. Najveći rizik generiranja za hrvatske nišne stavke je uvjerljiv, a netočan opis. Na primjer, model može „znati” žanr izvođača kojeg zapravo ne poznaje. To se provjerava na uzorku.

Plan kaže „generirati za stavke bez opisa i žanrova”. Predlažem da generiramo za **sve** stavke, ali označeno. Bodovi na osima i obilježja trebaju svim stavkama da bi bile usporedive. Inače bi strane stavke bile opisane jednim postupkom, a hrvatske drugim, što stvara upravo pristranost koju želimo izbjeći.

## Provjera prije pilota

### Uzorak za provjeru pretrage i pokrivenosti

Prvi uzorak naginjao je poznatim imenima. Za ovu provjeru predlažem uzorak bliži stvarnim listama:

| Domena | Hrvatsko i regionalno | Strano | Ukupno |
|---|---|---|---|
| Glazba (pjesme) | 50, od toga barem 15 novijih ili nišnih (hip-hop, elektronika, pjesme nakon 2018.) | 50, uključujući nišne | 100 |
| Film | 30, od toga barem 10 prije 1991. i 5 dokumentarnih | 30 | 60 |

Najbolji izvor uzorka su stvarni favoriti. Ako 5–10 budućih sudionika unaprijed pošalje po 5 pjesama i 5 filmova, uzorak će odražavati njihov ukus, a ne naš (otvoreno pitanje 9).

Za pjesme se dodatno mjeri:
- jesu li sve snimke iste pjesme ispravno spojene u jednu
- ima li pjesma Last.fm oznake
- ima li pjesma poveznicu za slušanje (za playlistu)

### Što mjerimo

| Mjera | Prag za nastavak |
|---|---|
| Stavka postoji u katalogu | ≥ 95% stranih, ≥ 85% hrvatskih |
| Točna stavka u prva 3 rezultata pretrage | ≥ 90% u obje skupine |
| Stavka ima barem jedan žanr ili stil iz otvorenog izvora | glazba ≥ 80% hrvatskih; film: mjeri se, ali ne blokira |
| Stavka ima tekst za opis (Wikipedija) | mjeri se, ne blokira |
| Generirani opis i obilježja su točni (ručna ocjena) | ≥ 85% stavki bez ozbiljne greške, posebno za hrvatske |

Pragovi su moj prijedlog i otvoreni su za raspravu.

### Ručna provjera generiranja (oko 50 stavki)

- 25 glazba i 25 film, pola hrvatskih i pola stranih
- Za svaku stavku ocjenjuju se opis (točno, djelomično, netočno), obilježja (koliko ih je pogrešno) i bodovi na osima (imaju li smisla)
- Idealno ocjenjuju dvije osobe, od kojih bar jedna dobro poznaje hrvatsku scenu

## Otvorena pitanja

Odlučeno: 1 (pjesme), 3 (MusicBrainz, Last.fm, TMDB), 4 (ručni unos zasad da), 5 (bez prednosti), 10 (bez lokalnih žanrova).

2. **Vrste filmova:** ulaze li dokumentarni, animirani i kratki filmovi? Prijedlog: dokumentarni i animirani da, kratki ne.
6. **Tekst Wikipedije:** smijemo li ga koristiti kao ulaz za generirane opise i vektore (CC BY-SA)? Ili samo za pilot, uz pravnu provjeru prije proizvoda?
7. **Veličina kataloga za pilot:** cijeli Discogs i Wikidata (milijuni stavki) ili podskup? Na primjer svi izvođači i filmovi s Wikidata stavkom i barem minimalnom popularnošću, plus sve hrvatske i regionalne stavke.
8. **Slike:** prikazuju li se omoti i plakati u pilotu (preko Wikidata ili Commons slika gdje postoje) ili je lista bez slika?
9. **Uzorak za provjeru:** smijem li predložiti da 5–10 budućih sudionika unaprijed pošalje po nekoliko favorita?
11. **Generiranje za sve stavke:** slažeš li se da generiramo opise i obilježja za sve stavke, uz oznaku, a ne samo za one bez opisa?
12. **Što znači „postaje playlista”?** a) Lista se može preslušati ili izvesti kao playlista (Spotify, YouTube), b) sudionik uvozi postojeću playlistu umjesto da pjesme traži jednu po jednu, ili oboje? Uvoz mijenja odluku „liste se slažu pretragom kataloga” i ovisi o tome što Spotify i YouTube još dopuštaju.
13. **Zvučna obilježja pjesme:** AcousticBrainz (ako je dostupan, ali bez pjesama nakon 2022.), vlastita analiza 30-sekundnih isječaka ili bez zvučnih obilježja? Prijedlog: prvo izmjeriti AcousticBrainz, a vlastitu analizu zadržati kao varijantu za usporedbu u pilotu.
14. **Duljina liste pjesama:** plan predviđa oko 10 stavki po domeni. Playlista je obično dulja, a pjesma nosi manje signala od izvođača. Prijedlog: 15–30 pjesama, s najmanje 10.
15. **Uživo i akustične verzije:** jesu li ista pjesma kao studijska verzija? Prijedlog: jesu. Remiksi i obrade su zasebne pjesme.

## Što slijedi nakon pregleda

Kad odgovoriš na pitanja, dopunit ću dokument odlukama. Zatim slijede:
- rječnik žanrova i obilježja (prvi prijedlog za pregled)
- popis uzorka za provjeru
- mjerenja koja su ostala otvorena: podtipovi filmova, strani film za usporedbu, novi i digitalni izvođači na Discogsu, **Last.fm oznake pjesama i izvođača, TMDB ključne riječi i hrvatski sažeci za hrvatske stavke** (oba trebaju API ključ), dostupnost i pokrivenost AcousticBrainza
