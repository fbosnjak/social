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

Posljedica: **za glazbu je Discogs glavni izvor žanrova, a ne MusicBrainz. Za film je glavni izvor opisa tekst s Wikipedije i generirani opis, jer otvoreni žanrovi nisu dovoljni.**

## Načela koja vrijede za obje domene

Ova načela proizlaze iz već donesenih odluka i izvještaja „Katalozi i rječnik ukusa”.

- **Kanonski identitet stavke** je Wikidata QID. Ako ga nema, stavka dobiva vlastiti ID, uz MBID ili Discogs ID kao poveznicu. ID vlasničkog servisa (Spotify, IMDb, TMDB) nikad nije kanonski. Smije se spremiti samo kao poveznica.
- **Trajno se koriste samo otvoreni izvori:** Wikidata (CC0), MusicBrainz jezgra (CC0) i Discogs dumpovi (CC0). Tekst Wikipedije (CC BY-SA) koristi se uz uvjet koji je otvoreno pitanje 6.
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

**Prijedlog:** lista se sastoji od **izvođača**, a albumi su neobavezni. Žanr kao stavka ne ulazi u pilot. To je otvoreno pitanje 1.

### Izvori i polja

| Polje | Izvor | Licenca | Napomena |
|---|---|---|---|
| Identitet, nazivi, aliasi | Wikidata, MusicBrainz | CC0 | MusicBrainz ima više aliasa i pravopisnih varijanti |
| Vrsta (osoba ili sastav), država, godine aktivnosti | MusicBrainz, Wikidata | CC0 | Kod 2 od 22 izvođača iz uzorka država nije upisana na MusicBrainzu |
| Žanrovi i stilovi | **Discogs dumpovi** (stilovi izdanja) | CC0 | Glavni izvor. Agregiraju se na razinu izvođača (vidi dolje). |
| Žanr | Wikidata (P136) | CC0 | Dopuna. Obično samo jedan, grub. |
| Žanrovi i oznake | MusicBrainz | **CC BY-NC-SA** | Samo ako odlučimo da pilot smije koristiti nekomercijalne podatke (otvoreno pitanje 3) |
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

### Rječnik žanrova

Za sličnost trebamo jedan zajednički rječnik u koji se preslikavaju svi izvori. Discogs ima oko 15 žanrova i nekoliko stotina stilova **[broj neprovjeren]**. Wikidata i MusicBrainz imaju vlastite popise.

**Prijedlog:**
- Osnovica su Discogs stilovi, jer su najpotpuniji za hrvatske stavke.
- Uz njih ide preslikavanje Wikidatinih žanrova na Discogs stilove.
- Dodaju se lokalni žanrovi koji u međunarodnim popisima ne postoje ili su skriveni pod „Folk, World, & Country”: klapa, tamburica, zabavna glazba, etno, turbofolk, starogradska, dalmatinska šansona i drugi. Popis treba složiti zajedno.

## Film

### Što se može birati

| Vrsta stavke | Za | Protiv |
|---|---|---|
| **Film** | Prirodna jedinica. Dobar identitet na Wikidati. | Hrvatski filmovi imaju slab opis. |
| **Redatelj** | Jak signal autorskog ukusa („volim Lanthimosa”). | Rijeđi način razmišljanja kod većine ljudi. Miješa se s filmovima na istoj listi. |
| **Serijal ili franšiza** (npr. Gospodar prstenova) | Ljudi tako i misle o tome. | Nejasno je je li to jedna stavka ili više njih. |

**Prijedlog:** lista se sastoji samo od **filmova**. Redatelji i glumci ne mogu se birati, ali se koriste kao svojstva filma. Dokumentarni i animirani filmovi su dopušteni. Serije ostaju izvan opsega. To je otvoreno pitanje 2.

### Izvori i polja

| Polje | Izvor | Licenca | Napomena |
|---|---|---|---|
| Identitet, naslovi (hr, izvorni, en), aliasi | Wikidata | CC0 | Hrvatski naslov ima 61% hrvatskih filmova |
| Godina, trajanje, država, izvorni jezik | Wikidata (P577, P2047, P495, P364) | CC0 | |
| Redatelj, scenarist, glavni glumci, skladatelj | Wikidata (P57, P58, P161, P86) | CC0 | Redatelja ima 84% hrvatskih filmova. Ljudi su ujedno signal sličnosti. |
| Žanr | Wikidata (P136) | CC0 | Pregrub: kod hrvatskih filmova najčešće samo „drama” |
| Glavna tema, radnja smještena u | Wikidata (P921, P840) | CC0 | Kod stranih filmova češće, kod hrvatskih rijetko **[nije izmjereno]** |
| Opis i radnja | hr ili en Wikipedija | CC BY-SA | Glavni izvor teksta. Hrvatski članak ima 52% hrvatskih filmova. |
| Generirani opis, žanrovi i bodovi na osima | generirano iz gornjeg teksta | vlastito | Za sve stavke, ali označeno |
| Popularnost | Broj Wikipedija (sitelinks) | CC0 | Za hrvatske filmove 6–19, za poznate strane 80+ |
| Poveznice | IMDb (P345), TMDB (P4947) | samo ID | Za prikaz i povezivanje. Podaci se ne preuzimaju. |
| Plakat | – | – | Otvoreno pitanje 8 |

### Rječnik žanrova i obilježja

Wikidatini žanrovi ne razlikuju dovoljno. „Drama” opisuje i Zvizdan i Fine mrtve djevojke. Zato predlažem dva sloja:
1. **Žanrovi:** mali kontrolirani popis od oko 25–30 žanrova (drama, komedija, triler, horor, dokumentarni, animirani…), na koji se preslikavaju Wikidatini žanrovi.
2. **Obilježja:** opisni pojmovi koji razlikuju ukus unutar žanra, na primjer „spori ritam”, „društvena kritika”, „crni humor”, „poslijeratna tema”, „autorski”, „žanrovski”, „vizualno stiliziran”. Dobivaju se generiranjem iz teksta i pregledavaju na uzorku.

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
 ├─ hr/en Wikipedija (sitelinks) → tekst opisa, broj Wikipedija kao popularnost
 └─ IMDb, TMDB, Spotify, Deezer ID → samo poveznice
```

Stavka bez Wikidata QID-a (novi izvođač, mali film) povezuje se preko MBID-a ili Discogs ID-a i dobiva vlastiti ID. Prema prvom mjerenju, MusicBrainz ima vezu na Wikidatu kod 16 od 22, a Wikidata ima Discogs ID kod 19 od 20 hrvatskih izvođača.

### Zapis stavke

Opis sadržaja, bez odluke o tehnologiji:

| Grupa | Polja |
|---|---|
| Identitet | vlastiti ID, Wikidata QID, MBID, Discogs ID, domena, vrsta (izvođač, album, film) |
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
1. **Filtriranje po domeni i vrsti.** U glazbi se traže samo izvođači (i albumi), a u filmu samo filmovi.
2. **Bez dijakritika i s aliasima.** „sajeta” nalazi Šajetu, „kranksvester” Krankšvester, a „The High Sun” i „Zvizdan” isti film.
3. **Rangiranje.** Prednost imaju popularnije stavke, da duplikati i prazne stavke padnu niže. Treba odlučiti dobivaju li hrvatske i regionalne stavke blagu prednost (otvoreno pitanje 5).
4. **Razlikovni podaci u rezultatu.** Na primjer „Murina · film · 2021 · Antoneta Alamat Kusijanović” ili „Zaa · izvođač · Srbija · reggae, dub”.
5. **Kad stavke nema.** Sudionik može upisati naziv ručno. Stavka se označi i kasnije ručno poveže ili opiše. Ovako se izbjegava da sudionik odustane ili izabere krivu stavku (otvoreno pitanje 4).

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
| Glazba (izvođači) | 50, od toga barem 15 novijih ili nišnih (hip-hop, elektronika, izvođači nakon 2018.) | 50, uključujući nišne | 100 |
| Film | 30, od toga barem 10 prije 1991. i 5 dokumentarnih | 30 | 60 |

Najbolji izvor uzorka su stvarni favoriti. Ako 5–10 budućih sudionika unaprijed pošalje po 5 izvođača i 5 filmova, uzorak će odražavati njihov ukus, a ne naš (otvoreno pitanje 9).

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

## Otvorena pitanja za prvi krug

1. **Vrste stavki u glazbi:** samo izvođači; izvođači uz neobavezne albume; ili i pjesme? Prijedlog: izvođači, uz neobavezne albume.
2. **Vrste stavki u filmu:** samo filmovi ili i redatelji? Ulaze li kratki filmovi i dokumentarci? Prijedlog: samo filmovi, uključujući dokumentarne i animirane, bez kratkih.
3. **MusicBrainz žanrovi u pilotu:** pilot je nekomercijalno istraživanje. Smijemo li u pilotu usporediti rezultate s MusicBrainz žanrovima i bez njih? Ili se odmah držimo samo trajno dopuštenih izvora, da rezultati pilota vrijede i za proizvod? Prijedlog: samo trajno dopušteni izvori.
4. **Ručni unos:** smije li sudionik upisati stavku koje nema u katalogu? Prijedlog: smije. Takve stavke ručno povezujemo ili opisujemo prije analize.
5. **Prednost domaćim stavkama u pretrazi:** treba li hrvatskim i regionalnim stavkama dati blagu prednost u rangiranju?
6. **Tekst Wikipedije:** smijemo li ga koristiti kao ulaz za generirane opise i vektore (CC BY-SA)? Ili samo za pilot, uz pravnu provjeru prije proizvoda?
7. **Veličina kataloga za pilot:** cijeli Discogs i Wikidata (milijuni stavki) ili podskup? Na primjer svi izvođači i filmovi s Wikidata stavkom i barem minimalnom popularnošću, plus sve hrvatske i regionalne stavke.
8. **Slike:** prikazuju li se omoti i plakati u pilotu (preko Wikidata ili Commons slika gdje postoje) ili je lista bez slika?
9. **Uzorak za provjeru:** smijem li predložiti da 5–10 budućih sudionika unaprijed pošalje po nekoliko favorita?
10. **Lokalni žanrovi:** koje hrvatske i regionalne žanrove i scene svakako dodati u glazbeni rječnik? Imaš li popis ili da ja složim prijedlog?
11. **Generiranje za sve stavke:** slažeš li se da generiramo opise i obilježja za sve stavke, uz oznaku, a ne samo za one bez opisa?

## Što slijedi nakon pregleda

Kad odgovoriš na pitanja, dopunit ću dokument odlukama. Zatim slijede:
- rječnik žanrova i obilježja (prvi prijedlog za pregled)
- popis uzorka za provjeru
- mjerenja koja su ostala otvorena (podtipovi filmova, strani film za usporedbu, novi i digitalni izvođači na Discogsu)
