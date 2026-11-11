# Mjerenja za glazbu i film (drugi krug, 11. 11. 2026.)

Dopuna bilješke `hrvatska_pokrivenost.md`. Ovaj krug zatvara praznine koje su ostale za glazbu i film: licencu MusicBrainz žanrova, Discogs stilove za hrvatske izvođače i veličinu hrvatskog filmskog korpusa na Wikidati. Upiti su išli na javna sučelja bez ključa.

## 1. Licenca MusicBrainz žanrova: nisu CC0

MusicBrainz dijeli bazu na dva dijela ([MusicBrainz Database](https://musicbrainz.org/doc/MusicBrainz_Database); [Data License](https://musicbrainz.org/doc/About/Data_License)):
- **Jezgra (CC0):** izvođači, albumi (release group), izdanja, snimke, djela, aliasi, države, datumi, veze između entiteta i URL veze. Pod jezgru spada i **popis žanrova** (naziv, aliasi, MBID).
- **Dopunski podaci (CC BY-NC-SA 3.0):** korisničke bilješke, ocjene, statistike i **oznake, uključujući dodjelu žanrova** izvođačima i albumima.

Iz toga slijedi:
- Žanrovi koje MusicBrainz dodjeljuje izvođačima su nekomercijalni. Spadaju pod istu zabranu kao Last.fm.
- Popis žanrova kao rječnik jest CC0 i može poslužiti kao temelj vlastitog rječnika.
- Za komercijalnu uporabu dopunskih podataka MetaBrainz nudi licencu (cijena nije provjerena).

To potvrđuje oznaku **[neprovjereno]** iz izvještaja „Katalozi i rječnik ukusa”.

## 2. Discogs žanrovi i stilovi za hrvatske izvođače

**Metoda:**
1. Discogs ID izvođača dohvaćen je s Wikidate (P1953) za 24 imena iz prvog uzorka. Pronađeno je 21; nedostaju Gibonni, KUKU$ i Kries, vjerojatno zbog drugačijeg naziva stavke.
2. Za svakog izvođača dohvaćena su glavna izdanja (uloga „Main”) i poredana po broju korisnika koji ih imaju u kolekciji.
3. Za prva tri izdanja (master ili izdanje) zbrojeni su žanrovi i stilovi.

Izvor: [Discogs API](https://api.discogs.com/artists/578760/releases). Žanrovi i stilovi izdanja dio su mjesečnih CC0 dumpova.

| Izvođač | Žanrovi | Stilovi |
|---|---|---|
| Oliver Dragojević | Pop | Chanson |
| Severina | Folk/World/Country, Pop | Ballad, Europop, Vocal |
| Haustor | Funk/Soul, Hip Hop, Jazz, Reggae, Rock | Alternative Rock, Art Rock, Electro, Funk, New Wave, Reggae, Reggae-Pop |
| Prljavo kazalište | Pop, Rock | New Wave, Pop Rock, Rockabilly, Ska |
| Let 3 | Electronic, Folk/World/Country, Rock | Alternative Rock, Experimental, Folk Rock, Goth Rock, Noise Rock, Post-Punk, Punk |
| Ekatarina Velika | Rock | Alternative Rock, Art Rock, Goth Rock, New Wave, Pop Rock, Post-Punk |
| Bajaga i instruktori | Jazz, Pop, Reggae, Rock | Arena Rock, Ballad, Cool Jazz, New Wave, Pop Rock, Reggae |
| Dubioza kolektiv | Folk/World/Country, Hip Hop, Reggae, Rock | Alternative Rock, Folk Rock, Ragga HipHop, Reggae, Ska |
| Elemental | Hip Hop | Conscious, Jazzy Hip-Hop |
| Vojko V | Hip Hop | Conscious, Trap |
| Grše | Hip Hop | Trap |
| Krankšvester | Electronic, Funk/Soul, Hip Hop, Pop | 13 stilova, od Gabber i Drum n Bass do Schlager i Thug Rap |
| Jinx | Funk/Soul, Pop, Rock | Europop, Funk |
| Detour | Electronic, Pop, Rock | Acoustic, Ballad, Downtempo |
| Nipplepeople | Electronic, Pop | Deep House, Electro, Synth-pop |
| Šajeta | Reggae, Rock | Classic Rock, Pop Rock, Punk, Reggae-Pop |
| Mile Kekin | Folk/World/Country, Pop, Rock | Acoustic, Blues Rock, Folk Rock, Pop Rock |
| Baby Lasagna | Electronic, Pop, Rock | Alternative Metal, Alternative Rock, Europop |
| *Radiohead (kontrola)* | Electronic, Rock | Alternative Rock, Experimental, IDM, Indie Rock |
| *Arctic Monkeys (kontrola)* | Rock | Alternative Rock, Indie Rock |
| *Billie Eilish (kontrola)* | Electronic, Pop | Acoustic, Indie Pop |

**Nalazi:**
- **18 od 18** hrvatskih i regionalnih izvođača s Discogs ID-jem ima žanr i barem jedan stil. Za usporedbu: na MusicBrainzu je žanr imalo 7 od 22, a na Wikidati obično samo jedan grubi žanr.
- Discogs pokriva upravo ono što na MusicBrainzu nedostaje: hip-hop (Elemental, Vojko V, Grše) i elektroniku (Nipplepeople, Detour).
- Kod hrvatskih izvođača stilovi su jednako detaljni kao kod kontrolnih stavki.
- **Šum:** stilovi pripadaju izdanjima, a ne izvođaču. Kompilacije i suradnje unose stilove koji ne opisuju izvođača: Haustor „Hip Hop”, Bajaga „Cool Jazz”, Krankšvester „Schlager”. Profil izvođača zato treba graditi iz više izdanja, s pragom učestalosti, a ne iz unije stilova (zaključak).
- Uzorak je mali i naginje poznatim imenima. Pokrivenost novih izvođača koji su objavljivali samo digitalno vjerojatno je slabija (zaključak, nije izmjereno).

**Neprovjereno:** jesu li brojevi „ima u kolekciji / želi” (community stats) dio CC0 dumpova ili samo API-ja. Ako nisu, ne mogu biti trajni izvor popularnosti.

## 3. Hrvatski film na Wikidati

SPARQL upiti na [Wikidata Query Service](https://query.wikidata.org/). Brojevi vrijede za stavke kojima je izravni tip „film” (Q11424). Upit koji uključuje podtipove (dokumentarni, kratki, TV film) za Hrvatsku je završio greškom 502/504, pa ti brojevi nedostaju.

| Država podrijetla (P495) | Filmova | sa žanrom | s hr opisom | s člankom na hr Wikipediji | s IMDb ID | s redateljem | s hr nazivom |
|---|---|---|---|---|---|---|---|
| Hrvatska (Q224) | 394 | 310 (79%) | 47 (12%) | 206 (52%) | 362 (92%) | 329 (84%) | 239 (61%) |
| SFRJ (Q83286) | 199 | 177 | 1 | 26 | 195 | 182 | 30 |
| Jugoslavija (Q36704) | 1.207 | 945 | – | 232 | – | – | – |
| Srbija (Q403) | 559 | 415 | 10 | 46 | 530 | 432 | 65 |
| BiH (Q225) | 109 | 90 | 7 | 20 | 104 | 93 | 24 |

Uz Hrvatsku i obje oznake za Jugoslaviju (sa svim podtipovima za Jugoslaviju) ukupno je **2.020** različitih stavki.

Broj žanrova po hrvatskom filmu:

| Žanrova | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| Filmova | 84 | 249 | 39 | 16 | 5 | 1 |

Najčešći žanrovi hrvatskih filmova: drama (159), dokumentarni (60), komedija (36), triler (16), biografski (14), ratni (8).

**Nalazi:**
- Identitet je dobro pokriven: IMDb ID ima 92%, a redatelja 84% filmova.
- Žanr je preslab za profil. 84 filma nemaju žanr, a 249 ima jedan, najčešće „drama”. To ne razlikuje ukuse.
- Opis na hrvatskom ima samo 12% filmova. Članak na hrvatskoj Wikipediji ima 52%, pa je to glavni izvor teksta za opise.
- Jugoslavenski filmovi većinom su označeni državom Jugoslavija (Q36704), a ne SFRJ. Pretraga i pravilo „hrvatsko ili regionalno” moraju obuhvatiti obje oznake (zaključak).
- Broj od 394 hrvatska filma je donja granica, jer upit nije obuhvatio podtipove. HAVC-ov katalog od 2008. i Hrvatska kinoteka mogu pokazati koliko nedostaje (neizmjereno).

## 4. Pjesme na MusicBrainzu

Provjera nakon odluke da se u glazbi biraju pjesme. Pretraga snimki (`recording:"…" AND artist:"…"`) i dohvat prvog pogotka s `inc=genres+tags+isrcs` ([MusicBrainz API](https://musicbrainz.org/doc/MusicBrainz_API)).

| Pjesma | Pogodaka (snimki) | Žanrovi snimke | Oznake | ISRC |
|---|---|---|---|---|
| Baby Lasagna – Rim Tim Tagi Dim | 6 | – | 0 | 1 |
| Prljavo kazalište – Mojoj majci | 13 | – | 0 | 0 |
| Oliver Dragojević – Cesarica | 15 | – | 0 | 1 |
| Radiohead – Creep | 194 | – | 0 | 0 |
| Arctic Monkeys – Do I Wanna Know? | 24 | – | 0 | 0 |
| Billie Eilish – bad guy | 37 | – | 0 | 1 |

Pet upita nije vratilo ništa: Ništa kontra Splita, Ena (Let 3), Ljubav ili mržnja (Elemental), Moja mala djevojčica (Detour) i Šuti moj dječače plavi (Gibonni). **To nije mjera pokrivenosti.** Naslove sam složio iz sjećanja i neki možda nisu točni ili pripadaju drugom izvođaču. Upit s točnim navodnicima osim toga promašuje pravopisne varijante.

**Nalazi:**
- Snimke nemaju žanrove ni oznake, ni kontrolne pjesme. Žanrovi na MusicBrainzu žive na izvođaču i albumu (release group). Pjesma ih mora naslijediti.
- Jedna pjesma ima mnogo snimki (Creep 194). Treba pravilo za kanonsku pjesmu, na primjer djelo (work) uz izvođača ili grupiranje po naslovu, izvođaču i ISRC-u.

**Zvučna obilježja:**
- Spotify je audio features zatvorio za nove aplikacije 27. 11. 2024. ([Spotify](https://developer.spotify.com/blog/2024-11-27-changes-to-the-web-api)).
- AcousticBrainz nudi dumpove obilježja za oko 29,5 milijuna prijava. Zadnja obavijest na stranici je iz srpnja 2022. ([AcousticBrainz download](https://acousticbrainz.org/download)). Jesu li dumpovi još dostupni i koliko pokrivaju hrvatske pjesme, nije provjereno.

## Što i dalje nije izmjereno
- Pokrivenost hrvatskih filmova kad se uključe podtipovi (dokumentarni, kratki, TV film).
- Usporedni brojevi za strani film (upit za Francusku je istekao).
- Pretraga Wikidate za višeznačne naslove nakon filtriranja po tipu (Murina, Zvizdan). Upiti su ograničeni zbog prevelikog broja zahtjeva (HTTP 429).
- Discogs za nove i nišne izvođače koji izdaju samo digitalno, te za izvođače bez Discogs ID-ja na Wikidati.
- Licenca Discogs podataka o popularnosti (kolekcije i liste želja).
