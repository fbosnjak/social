# Croatian / ex-Yu coverage in open catalogues (empirical sample, 2026-11-11)

**Sampling method (applies to all sections).** Hand-picked convenience sample from the brief, run on 2026-11-11 against live public read-only endpoints with keyless scripted requests:
- Music: 22 Croatian/regional artists + 3 international controls (Radiohead, Arctic Monkeys, Billie Eilish).
- Film: 9 Croatian films + 2 controls (Parasite, Oppenheimer).
- Series: 7 Croatian series + 2 controls (Breaking Bad, The Wire).
- Books: 14 Croatian titles + 2 controls (Normal People, Stoner).
- Podcasts: 5 names from the brief + 2 controls (Joe Rogan Experience, Huberman Lab).
- Games: 3 Croatian-developed Steam titles + 1 control (Hades).

A miss means the item was not found by **one search query**. Endpoint errors were retried, and a miss caused by HTTP 429 or 503 is reported as "not measured", not as absent. These are **small-sample estimates**: for n≈20, a 95% interval is roughly ±20 percentage points. The items lean towards well-known names, so real long-tail coverage is probably lower.

Several catalogues could not be measured:
- Last.fm: tag pages returned a bot-challenge page, and the API needs a key.
- Google Books: HTTP 429 on every call without a key.
- TMDB API, Podcast Index, IGDB and Spotify: all need keys or auth.
- Wikidata: under rate limiting during an outage. The query service printed "Aggressively rate-limiting to 1 req / min … during active wdqs outage", so some Wikidata cells are missing.

Endpoints used:
- MusicBrainz: `https://musicbrainz.org/ws/2/artist?query=…&fmt=json` and `/ws/2/artist/{mbid}?inc=tags+genres+url-rels`
- Deezer: `https://api.deezer.com/search/artist?q=…`
- Wikidata: `https://www.wikidata.org/w/api.php?action=wbsearchentities|wbgetentities` and `https://query.wikidata.org/sparql`
- IMDb suggestions: `https://v3.sg.media-imdb.com/suggestion/x/{q}.json`
- TMDB: website search
- Open Library: `https://openlibrary.org/search.json`
- Google Books: `https://www.googleapis.com/books/v1/volumes`
- Apple Podcasts: `https://itunes.apple.com/search?media=podcast&country=hr`
- Steam: `https://store.steampowered.com/api/storesearch` and `/api/appdetails`

## Q1. Coverage % and metadata richness per catalogue × domain (Croatian vs international controls)

### Takeaway
Croatian items are **found** at high rates in the big ID catalogues: MusicBrainz, Deezer, Wikidata and IMDb are each about 90–100% for this sample. **Metadata richness is much lower** than for international controls:
- MusicBrainz genres exist for only about 1/3 of Croatian artists, against 3/3 controls with 5+ genres and 20+ tags.
- Wikidata usually has exactly one coarse genre, against 3–9 for controls.
- Open Library subjects exist only for Croatian books that have been translated into English.
- Untranslated contemporary Croatian books have zero subjects.

### Cited Findings

**Music: MusicBrainz** (n=22 Croatian/regional)
- Coverage:
  - All 22 names returned a matching artist entity, about 95–100%.
  - "Zaa" was ambiguous. The hits were a Mexican trance DJ, a Serbian reggae-dub band "ZAA" and an Austrian rapper, so it is unclear which act the brief means.
  - Kries and KUKU$ have no country set.
  - The first quoted `artist:"…"` query missed 7 items (including control Arctic Monkeys). A plain query found all of them, so the misses were query/ratelimit artefacts, not coverage gaps.
  - Source: [MusicBrainz search API](https://musicbrainz.org/ws/2/artist?query=Haustor&fmt=json)
- Genres: only **7/22 (32%)** have any curated genre: Oliver Dragojević (pop), Prljavo kazalište (new wave, pop rock, punk, punk rock), Let 3 (alternative rock, post-punk), Krankšvester (gabber), EKV (new wave, post-punk, rock), Baby Lasagna (metal, metalcore, pop punk, rock, singer-songwriter), KUKU$ (trap).
  - Gibonni, Severina, Haustor, Dubioza kolektiv, Bajaga i Instruktori, Elemental, Jinx, Nipplepeople, Detour, Mile Kekin, Grše, Šajeta and Vojko V have none.
  - Controls: Radiohead has 27 tags and genres such as alternative rock and art rock. Billie Eilish has 22 tags, Arctic Monkeys 20, each with 5+ genres.
  - Source: [MusicBrainz artist lookup inc=genres+tags](https://musicbrainz.org/ws/2/artist/?fmt=json)
- Genre quality issue: MusicBrainz tags Krankšvester as "gabber", while Wikidata describes it as a "Croatian hip hop band". Sparse folksonomy tags on niche acts can be wrong. Sources: [MusicBrainz](https://musicbrainz.org/search?query=Kranksvester&type=artist), [Wikidata Q58207989](https://www.wikidata.org/wiki/Q58207989)
- Cross-IDs via MusicBrainz url-rels:
  - Spotify link: 12/22 (55%)
  - Wikidata link: 16/22 (73%)
  - Discogs link: about 19/22
  - Vojko V links only to Apple Music. The Croatian "Severina" entity found (5a8bf620, HR) has zero URL relations, which suggests a sparse duplicate.
  - Controls have 7–9 link types each.
  - Source: [MusicBrainz API](https://musicbrainz.org/doc/MusicBrainz_API)

**Music: Deezer** (existence only)
- All 22 names returned a top artist hit with the same name, for example Oliver Dragojević 140k fans, Dubioza Kolektiv 96k, Prljavo Kazalište 85k, Gibonni 61k, Šajeta 622, Kries 388.
- Two top hits look like the wrong or a duplicate entity: "Severina" with 4 fans and "Zaa" with 68 fans. Confident coverage is therefore about 20/22 (91%), and search ranking for common or short names is unreliable.
- Source: [Deezer search API](https://api.deezer.com/search/artist?q=Severina)

**Music: Wikidata** (20 searched, excluding Kries and Zaa)
- Found **20/20**.
- Genre P136 present in **16/20 (80%)**, but usually **one** value. Vojko V, Detour, Grše and KUKU$ have none. Controls: Radiohead 8, Arctic Monkeys 9.
- Croatian-language description present in 12/20. English description in 18/20 (KUKU$ and Šajeta have neither).
- Discogs ID: 19/20. Spotify ID: 8/20. Deezer ID: about 11/20. NSK authority ID: 2/20 (Elemental, Baby Lasagna).
- Source: [Wikidata API wbgetentities](https://www.wikidata.org/w/api.php?action=wbgetentities&ids=Q207134)

**Music: Last.fm**
- Not measured. `last.fm/music/{artist}/+tags` returned a 3 KB bot-challenge page for every artist, including the Radiohead control. The Last.fm API needs a key. Source: [Last.fm](https://www.last.fm/music/Gibonni/+tags)

**Film: IMDb** (n=9)
- **9/9 found**, all as feature films, usually under the English title: "One Song a Day Takes Mischief Away" (1970), "The Priest's Children", "The High Sun", "Murina", "Quit Staring at My Plate", "Safe Place", "Fine Dead Girls", "What Is a Man Without a Moustache?". A search on the Croatian title still found them.
- Source: [IMDb suggestion endpoint](https://v3.sg.media-imdb.com/suggestion/x/zvizdan.json)

**Film: Wikidata**
- 7/9 confirmed: Kad mrtvi zapjevaju, Tko pjeva zlo ne misli, Svećenikova djeca, Ne gledaj mi u pijat, Sigurno mjesto, Fine mrtve djevojke, Što je muškarac bez brkova.
- Zvizdan: rate-limited (429).
- "Murina": the search returned the bat genus, a disambiguation problem rather than confirmed absence.
- All 7 have a genre (1–3), an IMDb ID and a TMDB ID. Only 1/7 has a Croatian description. Sitelinks: 6–19 against 81 for the controls. Controls have 3–5 genres plus Rotten Tomatoes, Metacritic and main-subject data.
- Source: [Wikidata Q7093213](https://www.wikidata.org/wiki/Q7093213), [Wikidata Q2989550](https://www.wikidata.org/wiki/Q2989550)

**Film: TMDB** (website, spot checks)
- Zvizdan (2015) found with an overview.
- All 7 films found in Wikidata carry a TMDB ID, so TMDB coverage of the film sample is about 8/9 or better. This is indirect evidence.
- Source: [TMDB search Zvizdan](https://www.themoviedb.org/search?query=Zvizdan)

**Series: IMDb** (n=7)
- 6/7 clearly found as TV series: Novine (2016), Crno-bijeli svijet ("Black & White World", 2015), Bitange i princeze (2005), Gruntovčani (1975), Na terapiji, Uspjeh (2019).
- "Na terapiji" returned three different series (2009, 2011, 2013), so the right regional version needs disambiguation.
- "Kotlovina" returned only a 2011 **feature**. Either the brief's item is a film or the series is not indexed (unresolved).
- Source: [IMDb suggestion endpoint](https://v3.sg.media-imdb.com/suggestion/x/novine.json)

**Series: TMDB**
- Bitange i princeze found as a 2005 series with an overview, plus a 2004 film. Source: [TMDB TV search](https://www.themoviedb.org/search/tv?query=Bitange%20i%20princeze)

**Series: Wikidata**
- 4 confirmed: Novine (1 genre), Crno-bijeli svijet (0 genres), Gruntovčani (1 genre), Na terapiji (0 genres). All 4 have IMDb and TMDB-TV IDs.
- Bitange i princeze: rate-limited.
- "Uspjeh" and "Kotlovina": the search returned the common noun or the dish, a disambiguation problem.
- Controls: Breaking Bad has 5 genres and 103 sitelinks; The Wire has 3 genres.
- Source: [Wikidata Q27771149](https://www.wikidata.org/wiki/Q27771149)

**Books: Open Library** (14 Croatian queries)
- **10/14 found (71%)**.
- Not found: Čudnovate zgode šegrta Hlapića (0 hits for title + author with diacritics), Mraović's Pijavice, Slamnig's Neprijatelj. Priče iz davnine returned only a German study about it.
- Subjects exist **only for internationally translated works**:
  - Povratak Filipa Latinovicza: 5 subjects, 9 editions
  - Muzej bezuvjetne predaje: 15 subjects, 31 editions in 17 languages
  - Kultura laži: 8 subjects
  - Sarajevski Marlboro: 6 subjects, but the work is titled "Short stories"
  - Trieste: 19 subjects, English editions only
- Untranslated works have **0 subjects** and 1–4 editions: Hotel Zagorje, Črna mati zemla, Kiklop, Adio kauboju, Bolja polovica hrabrosti.
- Adio kauboju's language is recorded as `srp`, which is wrong.
- Goodreads ID present only on the translated works.
- Controls: Normal People has 40 subjects and 28 editions; Stoner 23 subjects and 49 editions.
- Source: [Open Library search API](https://openlibrary.org/search.json?q=Hotel+Zagorje+Bodrozic)

**Books: Wikidata works by author** (SPARQL, `?work wdt:P50 ?author`)

| Author | Works | With genre | With OL ID | With Goodreads | With hr description |
|---|---|---|---|---|---|
| Krleža | 24 | 12 | 1 | 2 | 7 |
| Jergović | 11 | 1 | 4 | 2 | 0 |
| Marinković | 5 | 0 | 0 | 0 | 2 |
| Ugrešić | 5 | 1 | 1 | 1 | 1 |
| Brlić-Mažuranić | 3 | 1 | 0 | 0 | 2 |
| Bodrožić | 3 | 0 | 0 | 0 | 0 |
| Drndić | 1 | 0 | 0 | 0 | 0 |
| Novak | 1 | 0 | 0 | 0 | 1 |
| Mraović | 1 | 0 | 0 | 0 | 0 |
| Slamnig | 0 | – | – | – | – |
| Savičević | 0 | – | – | – | – |

- Controls: Sally Rooney 18 works (3 with genre, 11 with OL ID); Elena Ferrante 135 works (3 with genre). Genre coverage is thin even for the controls.
- The only "Črna mati zemla" item found is the **theatre play** adaptation, not the novel.
- Source: [Wikidata Query Service](https://query.wikidata.org/)

**Books: Google Books**
- Not measured: HTTP 429 on every keyless request. Source: [Google Books API](https://www.googleapis.com/books/v1/volumes?q=intitle:Kiklop)

**Podcasts: Apple Podcasts** (search API, country=hr)
- **Pojačalo** found: 469 episodes, category Education, feed URL present.
- "Podcast Inkubator" not found as such. The closest hit is "Inkubator Dobre Glazbe" by Hrvatska diskografska udruga – IFPI Croatia (Music, 100 episodes); this may or may not be the intended show.
- "Nemam pojma", "Brkati" and "Bujica" returned unrelated shows. Bujica is a Z1 TV programme and may not exist as a podcast feed.
- Estimate: about 1–2/5 found (n is tiny, and some names in the brief may be inexact).
- Apple gives only a single coarse category plus parent categories, no tags. Controls (JRE: Comedy, 2000 eps; Huberman Lab: Health & Fitness) found, with similarly coarse categories.
- Source: [iTunes Search API](https://itunes.apple.com/search?media=podcast&country=hr&term=Poja%C4%8Dalo)

**Games: Steam**
- Serious Sam 3: BFE (developer Croteam): genres Action, Indie; 20 categories; short description.
- The Talos Principle (Croteam): Action, Adventure, Indie.
- SCUM (Gamepires): Action, Adventure, Indie, Massively Multiplayer.
- 3/3 found. Store genres are coarse; user tags are not in the appdetails API.
- Croatian games are mostly in English and international, so they behave like controls (Hades also found).
- Source: [Steam appdetails 257510](https://store.steampowered.com/api/appdetails?appids=257510)

### Inferences

**Summary table.** "Found" means identity coverage; "richness" means usable genre/tag/description data. All values are small-sample estimates.

| Domain | Catalogue | Croatian found | Croatian with genre/tags | Controls |
|---|---|---|---|---|
| Music | MusicBrainz | ~95–100% | ~32% | 100%, rich |
| Music | Deezer | ~91% (ranking errors) | n/a (not tested) | 100% |
| Music | Wikidata | ~100% | ~80%, usually 1 genre | 8–9 genres |
| Film | IMDb | 100% | (genres not measured) | 100% |
| Film | Wikidata | ≥78% | 100% of found, 1–3 genres | 3–5 genres |
| Series | IMDb | ~86% | – | 100% |
| Series | Wikidata | ≥57% (rate-limits) | 50% of found | 3–5 genres |
| Books | Open Library | ~71% | ~50% of found (translated works only) | 23–40 subjects |
| Books | Wikidata | sparse work items | ~0–50% by author | – |
| Podcasts | Apple | ~20–40% | 1 category | 100% |
| Games | Steam | 100% (internationally published) | coarse genres | same |

- Gaps concentrate in:
  - Hip-hop and electronic acts and newer artists: genres missing on MusicBrainz.
  - Untranslated contemporary Croatian books: no subjects anywhere tested.
  - Children's classics with diacritic titles.
  - Podcasts.
  - Short or ambiguous titles: Murina, Uspjeh, Kotlovina, Zaa, Severina. Search needs domain filtering and disambiguation.

### Gaps
- Last.fm tags, Google Books categories/descriptions, TMDB API genres/keywords/translated overviews, Podcast Index, IGDB, Spotify and Discogs genres/styles were not measured: keys, auth, bot challenges or rate limits.
- Wikidata cells for Zvizdan, Bitange i princeze and the games are missing because of the query-service outage rate limit.
- Kries and Zaa were not checked in Wikidata.
- Board games (BoardGameGeek) and Nanobit mobile titles (app stores) were not tested.

## Q2. Best catalogue per domain for Croatian content, and where the gaps are

### Takeaway
On this sample:
- **Music:** MusicBrainz plus Wikidata for identity and cross-IDs, Deezer for searchable presence. Genres are the gap.
- **Film/series:** IMDb, TMDB and Wikidata, linked by Wikidata's IMDb and TMDB IDs.
- **Books:** Open Library only for translated classics. No tested open source covers contemporary Croatian books well.
- **Podcasts:** Apple's search finds only some shows and gives coarse categories.
- **Games:** Steam covers Croatian studios' titles fine because they are international releases.

### Cited Findings
- Music: MusicBrainz identity coverage was about 95–100%, with Wikidata/Discogs/Spotify links on most mainstream entries. Niche or new acts are thinner: Vojko V's only link is Apple, Mile Kekin and Kries link only Discogs (plus AllMusic for Kries), Šajeta has no genre. Sources: [MusicBrainz API](https://musicbrainz.org/ws/2/artist?query=Vojko%20V&fmt=json), [Wikidata Q61129483](https://www.wikidata.org/wiki/Q61129483)
- Film: all 7 Wikidata-confirmed Croatian films carry IMDb and TMDB IDs. IMDb found 9/9 films. Sources: [Wikidata Q1287353](https://www.wikidata.org/wiki/Q1287353), [IMDb suggestion](https://v3.sg.media-imdb.com/suggestion/x/fine%20mrtve%20djevojke.json)
- Books:
  - Open Library has no subjects for Hotel Zagorje, Črna mati zemla, Kiklop or Adio kauboju, and no hit for Hlapić. Source: [Open Library](https://openlibrary.org/search.json?q=Crna+mati+zemla)
  - Wikidata has 0 work items for Slamnig and Savičević, and only 1 for Novak (the play). Source: [Wikidata Query Service](https://query.wikidata.org/)
- Podcasts: only Pojačalo was found reliably. Source: [iTunes Search API](https://itunes.apple.com/search?media=podcast&country=hr&term=Nemam%20pojma)
- Games: Croteam and Gamepires titles are on Steam with genres and descriptions. Source: [Steam appdetails 513710](https://store.steampowered.com/api/appdetails?appids=513710)

### Inferences
- Wikidata is the best **hub** for linking IDs across domains: it holds MBID, Discogs, Spotify and Deezer IDs for music and IMDb/TMDB IDs for screen. It is a weak **descriptor** for Croatian items: one genre, often no Croatian description.
- Regional (BA/RS) acts in the sample (Dubioza kolektiv, Bajaga, EKV) are covered about as well as Croatian mainstream acts.
- Search on Croatian titles works on IMDb and TMDB, but returned display titles are often English. The app should store both forms.

### Gaps
- No precise count for TMDB TV coverage or Croatian-language (hr-HR) overview availability; the API was not tested.
- Discogs search was not run directly. Its coverage is inferred from the high share of Discogs IDs in MusicBrainz and Wikidata.

## Q3. Do genres/tags exist in usable form, or would descriptions need to be generated/added?

### Takeaway
For international controls, genres and tags are rich enough to build taste profiles. For Croatian items they are mostly missing (music, books, podcasts) or a single coarse label (Wikidata, film/TV). A taste profile built only from catalogue metadata would be badly skewed. Expect to add or generate genres, tags and short descriptions for a large share of Croatian items, especially hip-hop/electronic artists, contemporary literature and podcasts.

### Cited Findings
- MusicBrainz genres: 7/22 Croatian artists against 3/3 controls with 5+ genres and 20+ tags. Source: [MusicBrainz API](https://musicbrainz.org/doc/MusicBrainz_API)
- Wikidata genre P136: 16/20 Croatian artists, usually 1 value, against 8–9 for controls. Croatian description present in 12/20 artists and 1/7 films. Source: [Wikidata Q44190 (Radiohead)](https://www.wikidata.org/wiki/Q44190), [Wikidata Q2625177 (Haustor)](https://www.wikidata.org/wiki/Q2625177)
- Open Library: 0 subjects on 5 of 10 Croatian titles found, against 23–40 for controls. Source: [Open Library](https://openlibrary.org/search.json?q=Kiklop+Marinkovic)
- Apple Podcasts: one primary category per show, even for the controls. Source: [iTunes Search API](https://itunes.apple.com/search?media=podcast&term=Huberman%20Lab)
- Example of a wrong niche tag: Krankšvester is "gabber" on MusicBrainz but "Croatian hip hop band" on Wikidata, which lists 5 genres. Source: [Wikidata Q58207989](https://www.wikidata.org/wiki/Q58207989)

### Inferences
- Croatian Wikipedia articles exist for most sampled artists, films and series (hrwiki sitelinks seen on nearly all music and film items), and could be a source text for generated descriptions or tags. Check the licence first (see Q4).
- Map genre vocabularies (MusicBrainz genres, Wikidata P136, Steam genres, Apple categories) onto one in-app taxonomy. Croatia-specific genres such as klapa, tamburica, "zabavna glazba" and "turbofolk" are probably under-represented in international taxonomies. This is a hypothesis from background knowledge and was not verified in the sample.

### Gaps
- Not measured: whether TMDB keywords or genres and Last.fm tags fill this gap for Croatian items.

## Q4. Croatian-specific data sources usable as a supplement, and their licences

### Takeaway
- **HAVC** publishes a year-by-year Croatian film catalogue covering films since 2008. It is a web catalogue with no stated open licence; inquiries go to katalog@havc.hr.
- Older films: Hrvatska kinoteka and the Hrvatski filmski savez database.
- **NSK**: no stated open licence or linked-data release was found.
- Not researched: HDS ZAMP and Discogs' Croatian label coverage. Licences below marked "background" are unverified.

### Cited Findings
- HAVC Croatian film catalogue: filterable by year, lists titles and directors, covers production since HAVC's founding in 2008. Older films are referred to Hrvatska kinoteka. Contact katalog@havc.hr. Source: [HAVC Croatian film catalogue](https://havc.hr/eng/croatian-film/croatian-film-catalogue?year=2019), [HAVC about](https://havc.hr/eng/about-us)
- A HAVC page mentions that Hrvatski filmski savez has developed an online Croatian film database; the link was not found. Source: [HAVC site content](https://havc.hr/eng/site-content)
- NSK: the search found no NSK page stating a CC0 or other open licence for its bibliographic or linked data. Comparable national libraries do publish under CC0, for example the German National Library's Linked Data Service and the Czech National Library. Source: [DNB Linked Data Service](https://www.dnb.de/EN/lds), [Wikimedia ČR on Czech National Library CC0](https://www.wikimedia.cz/en/2022/05/16/the-national-library-has-made-its-databases-available-under-cc0-license/)
- NSK authority IDs (Wikidata P1375) already appear on some Wikidata items (Elemental, Baby Lasagna, the city of Trieste), so NSK records are partly reachable via Wikidata. Source: [Wikidata Q124157590](https://www.wikidata.org/wiki/Q124157590)

### Inferences
- Licences, from background knowledge, **unverified in this session**:
  - Wikidata: CC0.
  - MusicBrainz core data: CC0; tags and annotations are under a separate non-commercial licence.
  - Discogs monthly data dumps: CC0.
  - Croatian Wikipedia text: CC BY-SA.
  - TMDB API: requires a key and attribution, with commercial-use terms.
  - Open Library data: openly available.
- HAVC and NSK would need direct contact to confirm reuse terms.

### Gaps
- HDS ZAMP repertoire search: not researched (existence, API, licence unknown).
- Discogs label pages for Croatia Records and Dallas Records: not fetched, so no counts.
- NSK linked data / OAI-PMH endpoint and licence: not confirmed.
- HAVC data licence and export format: not stated in the pages found.
