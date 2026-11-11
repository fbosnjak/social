# Rječnik nemedijskih domena: representing taste in hobbies, sport, food & drink, art and lifestyle

Research date: 2026-11-11. About 20 search and fetch calls were made. Items marked **[UNVERIFIED]** are background knowledge that was not confirmed in this session and need a check before anyone relies on them.

## 1. Existing taxonomies and vocabularies usable as item catalogues

### Takeaway
No single catalogue covers all non-media domains. The best combination is Wikidata as the multilingual backbone, because it has Croatian labels for common concepts. Domain vocabularies then fill in depth: Getty AAT/ULAN for art (ODC-By), BJCP and Untappd for beer, OSM `sport=*`/`cuisine=*` for activity and cuisine keys, and Europeana for artworks (CC0 metadata). Advertising taxonomies (IAB) are useful only as a coarse top level.

### Cited Findings
- **Wikidata "hobby" (Q47728):** the English description is "regular leisure activity that is done usually for enjoyment". It has a Croatian label, **"hobi"**, and labels in 115 languages. P279 points to Q184872 (normal rank) — [Wikidata Q47728 JSON](https://www.wikidata.org/wiki/Special:EntityData/Q47728.json)
- **Wikidata "cuisine" item:** Q1778821. Croatian cuisine is Q1789628, Mediterranean cuisine Q934309 and Italian cuisine Q192786 — [Wikidata Q1778821](https://www.wikidata.org/wiki/Q1778821), [Q934309](https://www.wikidata.org/wiki/Q934309), [Q192786](https://www.wikidata.org/wiki/Q192786)
- **Croatian coverage in Wikidata (lexemes, not item labels):** 45% in 2023, against a target of 60% — [Simia, Wikidata lexicographic data coverage for Croatian 2023](https://simia.net/wiki/Wikidata_lexicographic_data_coverage_for_Croatian_in_2023). A prior study found that Wikidata knowledge is concentrated in a few languages — [arXiv 2206.08709](https://arxiv.org/pdf/2206.08709v1.pdf)
- **Food items in other languages:** WikiFCD plus Wikidata can supply non-English labels for food items — [CEUR-WS Vol-3637 paper10](https://ceur-ws.org/Vol-3637/paper10.pdf)
- **Getty vocabularies (AAT, TGN, ULAN):** all free to use under **ODC-By 1.0**. They are available as LOD at vocab.getty.edu in JSON, RDF, Turtle and N-Triples, with full N-Triples dumps refreshed monthly and a SPARQL endpoint. The older XML web services have been discontinued. Getty supplies a template attribution line naming the J. Paul Getty Trust and the Getty Research Institute — [Getty Data & Services](https://www.getty.edu/research-conservation/tools-databases/vocabularies/data-services); [Getty download center](https://getty.edu/research/tools/vocabularies/obtain/download.html)
- **ULAN as LOD:** released April 2015 as the third vocabulary after AAT and TGN, with more than 650,000 names — [Getty news](https://www.getty.edu/news/getty-union-list-of-artist-names-ulan-linked-open-data); [InfoDocket 2015-04-30](https://www.infodocket.com/2015/04/30/getty-research-releases-union-list-of-artist-names-ulan-as-linked-open-data-lod-over-650000-names-included/)
- **Europeana:** all metadata is **CC0 1.0** and the API metadata is also CC0. Search runs through `api.europeana.eu/record/v2/search.json` (API key required), and bulk ZIP dumps are on an FTP server. It holds more than 50 million objects (2024) from more than 3,000 institutions — [Europeana terms of use](https://europeana.eu/en/rights/terms-of-use); [Europeana API FAQ](https://europeana.atlassian.net/wiki/x/AYCyj); [Wikipedia: Europeana](https://en.wikipedia.org/wiki/Europeana). Media files carry their own per-item rights statements, separate from the metadata licence **[UNVERIFIED detail]**.
- **IAB Content Taxonomy v2.0 (2017):** top-level tiers include "Food & Drink" (subcategories include cooking, desserts & baking, dining out) and "Hobbies & Interests" — [IAB Content Taxonomy T1/T2 public review PDF](https://www.iab.com/wp-content/uploads/2017/01/Content_Taxonomy_T1-T2_for_Public_Review.pdf.pdf). Audience Taxonomy 1.1 is reportedly about 1,600 nodes across demographic, interest and purchase-intent branches, but that comes from a third party — [daitics reference](https://daitics-docs.pages.dev/api/iab-reference); official page: [IAB audience taxonomy](https://iab.com/guidelines/audience-taxonomy). Third-party segment IDs (e.g. "Foodies IAB 692") did not match the official structure and should be treated as unreliable.
- **OSM `sport=*`:** a non-physical tag for "categorising any specific sport", meant to sit on a physical feature (`leisure=pitch`, `leisure=sports_centre`, `leisure=stadium`, `leisure=track`). The wiki lists **217** documented values, including `climbing` (natural sites and artificial walls) — [OSM wiki Key:sport](https://wiki.openstreetmap.org/wiki/Key:sport). This is a good list of locally practised sports and maps to venues. OSM data is ODbL **[UNVERIFIED in this session; widely known]**, and `cuisine=*` is a similar semicolon-separated key on restaurants **[UNVERIFIED]**.
- **Strava `sport_type`:** CamelCase enum values such as Run, TrailRun, MountainBikeRide, Ride, VirtualRide, WeightTraining, Windsurf, Yoga, Tennis and Walk. The older `type` field is deprecated in favour of `sport_type` — [Strava API v3 reference](https://developers.strava.com/docs/reference/); [Strava uploads docs](https://developers.strava.com/docs/uploads/); [stravalib model](https://stravalib.readthedocs.io/en/v2.2/_modules/stravalib/strava_model.html). Strava has added non-endurance modes (padel, dance, basketball and others) — [T3](https://www.t3.com/active/strava-new-sports-modes-padel-dance-basketball-etc)
- **BJCP 2021 Beer Style Guidelines:** numbered categories 1–34 (34 = Specialty Beer). It is a minor revision of 2015 and the current edition — [BJCP intro to 2021 guidelines](https://www.bjcp.org/beer-styles/introduction-to-the-2021-guidelines/); [styles and categories](https://styles.bjcp.org/bjcp-2021-beer/introduction-to-the-2021-guidelines/styles-and-categories). Licence and reuse terms are not checked **[GAP]**.
- **Untappd styles:** no official count was found. A third-party README says 224, and an older forum post says 144. Untappd adds styles yearly and in 2025 expanded beyond beer — [The Drinks Business, Nov 2025](https://www.thedrinksbusiness.com/2025/11/untappd-expands-beyond-the-beer-category/); [Untappd API v4](https://untappd.com/api/docs/v4)
- **Meetup topics:** categories contain topics, each with an id, name and urlkey. Third-party scrapers report about 20 current official categories, while a 2015 analysis cited 33, so the count has changed over time. A v3 "categories/find topics" API existed, and a GraphQL schema now exists — [hexdocs Meetup.V3.Topics](https://hexdocs.pm/meetup/Meetup.V3.Topics.html); [Meetup GraphQL schema](https://meetup.com/api/schema); [archived v2 docs](https://web-wp.archive.org/web/20140924045939/http:/www.meetup.com/meetup_api/docs/:urlname/topics/)
- Not checked in this session (all **[UNVERIFIED]**): Eventbrite categories (a short list of about 20 categories with subcategories, e.g. "Sports & Fitness", "Food & Drink", "Performing & Visual Arts", "Hobbies & Special Interest"); ESCO, which is occupations and skills under EU terms and suits hobbies poorly; schema.org, which has `SportsEvent`, `TheaterEvent`, `DanceEvent`, `ExhibitionEvent`, `FoodEvent`, `VisualArtsEvent`, `Restaurant.servesCuisine` and `Person.knowsAbout`, and is useful for event types, not taste items; FoodOn, an OBO ontology under CC-BY 4.0 that is very granular and food-safety oriented, with weak Croatian labels; TasteAtlas, which is proprietary with no open licence; Michelin cuisine labels, which are proprietary; Google Product Taxonomy, which is shopping-oriented; and Facebook/Meta "Interests" ad targeting, which has historically held tens of thousands of entries and from which Meta removed many sensitive interests in 2022.

### Inferences
- A workable stack per domain:
  - **Sport/hobby:** Wikidata (Q349 sport, Q47728 hobby subclasses), cross-checked against OSM `sport=*` and Strava `sport_type` for "what people actually do".
  - **Food & drink:** Wikidata cuisines and dishes, plus BJCP or Untappd style names for beer and Wikidata for wine varieties.
  - **Visual art and architecture:** Wikidata, with ULAN for artists and AAT for movements and styles (attribution required), and Europeana for concrete works.
  - **Theatre and dance:** Wikidata, covering genres, companies and venues.
- IAB, Meetup and Eventbrite categories are useful as a top two-tier skeleton for UI grouping and event typing, but too coarse for taste similarity.
- Prefer CC0 sources (Wikidata, Europeana metadata) for the core. ODC-By (Getty) is fine with attribution. Proprietary sources (TasteAtlas, Michelin, Untappd catalogue) should not be copied as catalogues.

### Gaps
- No exact count of Croatian labels for subclasses of sport, hobby, dish or cuisine in Wikidata. A SPARQL count was not run, because plan mode allows read-only actions only.
- Europeana's Croatian object count is not verified; a COUNTRY facet query is needed. BJCP licence terms and Eventbrite's official category list were not fetched.

## 2. Granularity: categories vs specific entities

### Takeaway
Use a hierarchy (domain → category → subcategory → entity) and let users pick at any level. Similarity should then roll entities up to their ancestors. Specific entities such as a named painter, restaurant or football club carry strong identity signal but rarely overlap between users. Categories overlap often but discriminate weakly.

### Cited Findings
- Pinterest's Interest Taxonomy is a hierarchical parent-child tree of topics and entities. Taxonomy granularity affects the accuracy of Pin2Interest predictions — [Pinterest Engineering: Interest Taxonomy KG](https://medium.com/pinterest-engineering/interest-taxonomy-a-knowledge-graph-management-system-for-content-understanding-at-pinterest-a6ae75c203fd); [knowledgespeak summary](https://www.knowledgespeak.com/?p=63207)
- Pinterest also builds taxonomies quickly from discovered dynamic "μ-topics", a bottom-up complement to a curated tree — [Mahabal et al. 2023, arXiv 2301.12520](https://arxiv.org/abs/2301.12520v1)
- In art-preference research, personality correlates were **stronger when art was classified by consensual (lay) ratings than by researcher-led or art-historical taxonomies** — Chamorro-Premuzic et al. 2010, *Psychology of Aesthetics, Creativity, and the Arts* — [Goldsmiths research](https://research.gold.ac.uk/id/eprint/5014)
- In folksonomies, one tag can carry several meanings (ambiguity) and several tags can mean the same thing (redundancy). Both problems hurt tag-recommender performance — [RecSys 2009, ACM 10.1145/1639714.1639724](https://www.springerprofessional.de/doi/10.1145/1639714.1639724)

### Inferences
- Recommended depth: about three levels plus entities, e.g. *Sport → Penjanje → Bouldering*, *Hrana → Japanska kuhinja → Ramen*, *Umjetnost → Moderna umjetnost → Ekspresionizam → [Egon Schiele]*, *Sport → Nogomet → [HNK Hajduk Split]*.
- Similarity: compute overlap at each ancestor level with an IDF-style weighting, so that rare shared items count more. Two Hajduk fans match strongly, and a Hajduk fan and a Dinamo fan still share "nogomet / navijanje".
- The Chamorro-Premuzic finding argues for grouping items by perceived character (e.g. "intense", "contemplative", "social") rather than only by formal taxonomy. This supports projecting items onto cross-domain dimensions (Section 4).
- Local entities (restaurants, clubs, venues, festivals) matter most for an events network and appear least in global vocabularies. They need Wikidata items where those exist, or else an app-internal entity linked to OSM nodes.

### Gaps
- No empirical study was found that directly compares entity-level and category-level interest similarity for leisure domains in a social or dating network.

## 3. How products model these interests

### Takeaway
Products use one of two patterns. The first is a small, flat, curated badge set (Bumble: about 150 badges, pick up to 5). The second is a large tag vocabulary matched to text (Foursquare Tastes: about 25,000 tastes). Pinterest and Meta sit in between, with a large hierarchical interest graph inferred from behaviour. Domain-specific apps (Strava, Untappd) use narrow, rich enums.

### Cited Findings
- **Foursquare Tastes (Foursquare 8.0, launched 6–7 Aug 2014):** users pick from words and phrases ("tastes"). About 25,000 tastes were available at launch, and users added more than 15 million in the first 24 hours. NLP matched tastes to venue tips that mention them, to personalise recommendations — [TheNextWeb 2014-08-06](https://thenextweb.com/news/foursquare-launches-redesigned-mobile-app-focused-location-based-recommendations); [TheNextWeb 2014-08-07](https://thenextweb.com/news/new-foursquare-app-already-15-million-user-added-tastes); [PhoneArena](https://www.phonearena.com/news/The-new-Foursquare-collects-15-million-tastes-in-its-first-24-hours_id59116); [Phys.org/AFP](https://phys.org/news/2014-08-foursquare-unveils-revamped-app.html). A "Taste Map" of popular tastes was published on 21 Oct 2014 — [Foursquare Direct](https://medium.com/foursquare-direct/exploring-the-foursquare-taste-map-52e743427001)
- **Bumble Interest Badges:** pick up to 5 from more than 150 interests covering sport, creative hobbies, going out vs staying in, film/TV and music. Examples are Coffee, Pets, Wine and Sweet Tooth. Bumble says 90% of members have at least 2 badges and 60% have all 5. Badges can be used to filter matches by a shared interest. Six self-care badges were added later (Therapy, Mindfulness, Deep chats, Nutrition, Sleeping well, Time offline) — [Bumble: badges](https://bumble.com/the-buzz/bumble-badges); [Bumble: filter by interest badges](https://bumble.com/the-buzz/filter-by-interest-badges); [Bumble: self-care badges](https://bumble.com/the-buzz/bumble-self-care-badge); [TechMoran 2021-04-16](https://techmoran.com/2021/04/16/bumble-has-added-a-new-feature-that-will-help-people-share-more-details-of-their-personalities/)
- **Pinterest:** Pin2Interest assigns taxonomy interests to more than 200 billion Pins. The output feeds user2interest (user interest inference), which drives ads and recommendations. The Pinterest Taxonomy is engineered as an OWL ontology and forms the core of the "Pinterest Taste Graph" — [Pin2Interest](https://medium.com/pinterest-engineering/pin2interest-a-scalable-system-for-content-classification-41a586675ee7); [arXiv 1907.02106](https://ar5iv.arxiv.org/html/1907.02106); [VentureBeat](https://venturebeat.com/business/pinterest-details-the-ai-and-taxonomy-systems-underpinning-its-trends-feature)
- **Strava:** activity-level `sport_type` enum (see Section 1). Interests are implicit in logged activity, not declared.
- **Untappd:** check-ins of specific beers, each tied to a style and brewery, so taste is entity-level with a style rollup — [Untappd API v4](https://untappd.com/api/docs/v4)
- **Meetup:** groups are tagged with topics inside categories, and users follow topics — [hexdocs Meetup.V3.Topics](https://hexdocs.pm/meetup/Meetup.V3.Topics.html)
- **Hinge:** no reliable primary source on its interest list was found. Hinge relies mostly on prompts (free text), not badges **[UNVERIFIED]**.
- Not verified in this session (all **[UNVERIFIED]**): Yelp has no declared taste profile and infers from reviews and bookmarks. Reddit communities act as a de facto folksonomy of interests (subreddits). Instagram/Meta "interests" are inferred ad-targeting categories that users can view in Ad Preferences.

### Inferences
- The "defines me" list model in this app is closest to Foursquare Tastes (declared and searchable) and Bumble badges (identity display). Bumble's high fill rate (60% with all 5) suggests that small caps produce deliberate, identity-laden choices. Foursquare's 15 million tastes in a day suggests that a large searchable vocabulary with suggestions lowers friction.
- Foursquare's taste↔tip NLP matching maps directly onto deriving event profiles from organiser descriptions. User-list items and event text should share one vocabulary.
- Pinterest shows that one curated hierarchical taxonomy across all domains is feasible at scale and benefits from bottom-up topic discovery to stay current.

### Gaps
- No published evaluation was found of how much Bumble badges or Foursquare Tastes improved match or recommendation quality.

## 4. Psychology and sociology: do leisure preferences map onto the cross-domain taste dimensions?

### Takeaway
The evidence base is consistent:
- **Openness** is the main correlate of art preference, leisure breadth and food adventurousness.
- **Sensation seeking** drives spicy food liking and some art preferences.
- **Extraversion and conscientiousness** (not openness) predict physical activity.
- Class-based **omnivorousness** extends into eating out and sport.

These line up reasonably with the dimensions Cerebral/Aesthetic (openness), Thrilling (sensation seeking), Communal (extraversion and social leisure) and Dark (disinhibition, neuroticism and abstract/pop art). The mapping is an inference, not an established model.

### Cited Findings
- **Spicy food:** Byrnes & Hayes 2013 (*Food Quality and Preference*), n ≈ 200 non-smokers aged 18–45, Arnett Inventory of Sensation Seeking. Sensation seeking correlated with liking spicy foods but not non-spicy control foods (r ≈ +0.50). Sensitivity to reward was moderate (r ≈ +0.23). Chili intake frequency was associated with both traits. As burn increased, liking among low sensation seekers dropped faster — [PMC3607321](https://pmc.ncbi.nlm.nih.gov/articles/PMC3607321); [PMC4316214](https://pmc.ncbi.nlm.nih.gov/articles/PMC4316214); [Penn State news](https://www.psu.edu/news/research/story/some-it-hot)
- **Food neophobia:**
  - Openness correlated negatively with food neophobia, r = −0.23 (corrected), with a genetic correlation of −0.39 in women — Silventoinen et al. 2011 twin study, via [PMC8794126](https://pmc.ncbi.nlm.nih.gov/articles/PMC8794126)
  - An Estonian Biobank study (n = 819) found food neophobia has 3 dimensions: approach motivation, avoidance motivation and preference flexibility. Approach tracked openness and extraversion, and avoidance tracked neuroticism — [UT dspace](https://dspace.ut.ee/bitstreams/6ba8d861-838e-4219-913d-6c722c68e5f3/download)
  - Food neophobia is not an immutable trait — [British Journal of Nutrition narrative review](https://resolve.cambridge.org/core/services/aop-cambridge-core/content/view/CB88B666559EDF453722527D67F47741/S0007114522003713a.pdf/a-narrative-review-on-food-neophobia-throughout-the-lifespan-relationships-with-dietary-behaviours-and-interventions-to-reduce-it.pdf)
- **Art preferences:**
  - Chamorro-Premuzic et al. 2009 (*British Journal of Psychology*), n = 91,692, rated 24 paintings (Cubist, Renaissance, Impressionist, Japanese). Openness was the strongest and most consistent correlate — [Goldsmiths: "Who art thou?"](https://research.gold.ac.uk/id/eprint/4898); [Scientific American](https://www.scientificamerican.com/article/you-are-what-you-like/)
  - Furnham & Chamorro-Premuzic 2004: openness relates to art experience and preference, not judgement ability — [EJOP](https://ejop.psychopen.eu/index.php/ejop/article/download/1479/1479.html?inline=1)
- **Furnham & Walker 2001** (*European Journal of Personality* 15:57–72), n = 121, 24 slides:
  - Thrill and adventure seeking went with liking representational art, and disinhibition with liking abstract and pop art.
  - Neuroticism went with abstract and pop art, and conscientiousness with representational art.
  - Openness went with all three styles, and agreeableness was negative for pop art.
  - **Sensation seeking explained more variance than the Big Five.**
  - A companion 2001 *PAID* paper failed to replicate several earlier links.
  - Sources: [Furnham & Walker 2001 PDF](https://e-l.unifi.it/pluginfile.php/929461/mod_folder/content/0/Furnham%20and%20Walker%20%282001%29.pdf); [CiNii](https://cir.nii.ac.jp/crid/1361699995871135872)
- **Physical activity:** Rhodes & Smith 2006 meta-analysis (*BJSM* 40:958–965; 33 studies, 35 samples) found extraversion r = 0.23, conscientiousness r = 0.20 and neuroticism r = −0.11. Openness and agreeableness were not associated — [PubMed 17124108](https://www.pubmed.gov/17124108/). A 2019 review of 26 studies (2013–2019) confirmed the extraversion link, especially for moderate-to-vigorous activity — [JYX](https://jyx.jyu.fi/bitstreams/cfdb2e85-ea4e-4bce-b24a-a54eafd5ce86/download)
- **Leisure and Big Five:**
  - In a 13-year panel, leisure participation was most strongly associated with openness between persons. Within persons, reciprocal effects with extraversion appeared only for overall participation and socializing — Sander et al. 2021, *Collabra: Psychology* — [EconStor/IDEAS](https://ideas.repec.org/a/zbw/espost/238209.html)
  - In the Estonian Biobank (n = 12,855), every Big Five trait was associated with at least one leisure activity — [UT dspace](https://dspace.ut.ee/server/api/core/bitstreams/9136210f-4ad9-4879-a154-0c61644a73d8/content)
  - Leisure interests and Big Five, citing Hansen, Dik & Zhou (2008) on the structure of leisure interests — [doi 10.1027/1614-0001/a000308](https://doi.org/10.1027/1614-0001/a000308)
- **Sociology:**
  - Bennett et al. 2009, *Culture, Class, Distinction* (British national survey). The exercised body (sport, exercise, diet) is a class marker, while eating at home shows little class distinction. Class patterns appear in preferred cuisines and in how often people eat out — summarised in [Sociology 2020, "Do omnivores perform class distinction?"](https://journals.sagepub.com/doi/full/10.1177/0038038520967257); [PMC5154390](https://pmc.ncbi.nlm.nih.gov/articles/PMC5154390)
  - Omnivores still perform distinction despite their openness to lowbrow forms — [Sociology 2020](https://journals.sagepub.com/doi/full/10.1177/0038038520967257)
- Background, **[UNVERIFIED in this session]**:
  - Hansen's Leisure Interest Questionnaire (Hansen & Scullard 2002) uses RIASEC-like leisure scales.
  - Tinsley's Paragraphs about Leisure (PAL) identifies about 44 leisure activity clusters and leisure needs (e.g. self-expression, companionship, power, compensation, security, service, intellectual stimulation, solitude).
  - Rentfrow, Goldberg & Zilca (2011) found 5 media-preference factors spanning books, TV, film and music: Communal, Aesthetic, Dark, Thrilling, Cerebral. These are the user's cross-domain dimensions. Leisure activities were not part of that model.

### Inferences
Proposed loadings of non-media items onto the existing dimensions. These are hypotheses to validate on in-app data.
- **Thrilling** (sensation seeking): extreme sports, climbing, MMA/combat sports, spicy food, rakija tasting nights, festivals.
- **Aesthetic** (openness, beauty): visual art (Impressionism, Japanese), architecture, ballet and contemporary dance, wine, fine dining.
- **Cerebral** (openness, intellect): chess, pub quiz, contemporary and conceptual art, museums, specialty coffee, craft-beer style nerdery.
- **Communal** (extraversion, agreeableness): team sports, fan clubs, folk dance, family-style food, social dancing, choir and klapa.
- **Dark** (disinhibition, neuroticism): punk and pop art, horror-themed events, nightlife.

Further inferences:
- **Arousal/valence/depth** can be assigned per item too. Spicy food and extreme sport are high arousal, and museums and yoga are low arousal with high depth.
- Physical-activity items are a **partly separate axis**: activity/energy level, driven by extraversion and conscientiousness. They should not be assumed to load on openness.
- Omnivorousness (breadth across brows and domains) could be computed per user as a profile feature (entropy across categories) rather than treated as a taste dimension.

### Gaps
- No published factor analysis jointly covers music, film, books, food, sport and art items in one model. This is the key empirical gap.
- Hansen's LIQ, Tinsley's PAL factors and the Rentfrow MUSIC/media model details were not fetched in this session.

## 5. User-generated vocabularies vs controlled vocabularies; multilingual labels

### Takeaway
Free tags capture long-tail and local items but suffer from ambiguity, redundancy and variant spellings. Normalising them to a controlled vocabulary is laborious and subjective. The usual fix is a hybrid: autocomplete from a controlled vocabulary with Croatian labels, a free-text fallback, and periodic clustering and merging of free entries into the vocabulary.

### Cited Findings
- Folksonomy ambiguity and redundancy impede tag recommenders. Clustering-based measures were tested on 3 real datasets — [RecSys 2009](https://www.springerprofessional.de/doi/10.1145/1639714.1639724)
- Lexical ambiguities (synonyms and homonyms) strongly affect structured knowledge extraction and tag-based recommendation quality — [BL record, 2012](https://eld.bl.uk/catalog/100046581520_0x000001)
- Mapping tag variants to a controlled vocabulary requires substantial, laborious and subjective normalisation (Delicious study) — [City University tech report](https://openaccess.city.ac.uk/id/eprint/1206/1/schleith_tech_report_June12.pdf)
- Social-tagging recommenders face noise from user heterogeneity, and a common mitigation is suggesting tags at entry time — [Social Tagging Recommender Systems chapter](https://www.ise.bgu.ac.il/faculty/liorr/recsyshb/chSocial.pdf)
- Last.fm artist tags were compared with collaborative-filtering artist similarity as a possible ground truth — [ISMIR 2007](https://archives.ismir.net/ismir2007/paper/000525.pdf). Large-scale folksonomy similarity measures — [arXiv 1207.6037](https://arxiv.org/pdf/1207.6037)
- Foursquare's controlled set of about 25,000 tastes plus NLP over free-text tips is a working hybrid — [TheNextWeb](https://thenextweb.com/news/new-foursquare-app-already-15-million-user-added-tastes)

### Inferences
- In Croatian, inflection (e.g. "planinarenje / planinarim / planinar"), diacritics (č ć š ž đ, often typed without them), dialect variants and English loanwords ("bouldering", "craft pivo") make free-text normalisation harder than in English. The pipeline should store the language-independent item ID, show hr and en labels with aliases, and fold diacritics for search.
- Wikidata aliases (`skos:altLabel`) can hold both Croatian and English variants.

### Gaps
- No study of Croatian-language tag normalisation was found.

## 6. Local context: Croatian hobbies, foods and sports; Wikidata Croatian labels

### Takeaway
Croatian labels exist for core concepts (e.g. "hobi" for Q47728), but coverage of fine-grained local items is unverified. Expect gaps for niche dishes, local clubs and regional games. A seeded local list curated in-app and contributed back to Wikidata is likely necessary.

### Cited Findings
- Q47728 has the Croatian label "hobi" among labels in 115 languages — [Wikidata JSON](https://www.wikidata.org/wiki/Special:EntityData/Q47728.json)
- Croatian cuisine is item Q1789628 — [Wikidata cuisine](https://www.wikidata.org/wiki/Q1778821)
- Croatian lexeme coverage was 45% in 2023 — [Simia](https://simia.net/wiki/Wikidata_lexicographic_data_coverage_for_Croatian_in_2023)
- Croatian Wikipedia participation in CEE Spring — [Meta-Wiki CEE Spring 2017 Croatia](https://meta.wikimedia.org/wiki/Wikimedia_CEE_Spring_2017/Structure/Statistics/Croatia)

### Seed candidates (all [UNVERIFIED]; check Wikidata hr labels via SPARQL)
- **Sport:**
  - Picigin (Split beach ball game)
  - Water polo (vaterpolo)
  - Handball (rukomet)
  - Basketball (košarka)
  - Football clubs (HNK Hajduk Split, GNK Dinamo Zagreb, HNK Rijeka) and fan groups (Torcida, BBB)
  - Sailing (jedrenje)
  - Rowing (veslanje)
  - Sea kayaking
  - Hiking (planinarenje, HPS trails)
  - Sport climbing (Paklenica)
  - Boćanje (bocce)
  - Sinjska alka (UNESCO intangible heritage)
  - Moreška (sword dance, Korčula)
  - Kitesurf and windsurf (Bol, Viganj)
- **Hobbies:**
  - Card games briškula and trešeta
  - Fishing (ribolov)
  - Mushroom picking (gljivarenje)
  - Truffle hunting (Istria)
  - Olive growing and harvest
  - Home rakija distilling
  - Klapa singing (overlaps with music)
- **Food:**
  - Peka
  - Soparnik
  - Štrukli
  - Pašticada
  - Fritule
  - Ćevapi
  - Black risotto (crni rižot)
  - Brudet
  - Kulen
  - Pršut
  - Paški sir
  - Istrian truffles
  - Ston oysters
  - Lika potatoes
- **Drink:**
  - Rakija types (travarica, šljivovica, biska, medica)
  - Pelinkovac
  - Maraschino
  - Wine varieties (Plavac mali, Graševina, Malvazija istarska, Pošip, Teran, Babić)
  - Gemišt/bevanda
  - Croatian craft breweries
- **Art and theatre:**
  - Naïve art (Hlebine school)
  - Ivan Meštrović
  - Dubrovnik Summer Festival
  - HNK theatres
  - Zagreb Dance Company
  - Lado ensemble (folk dance)
  - Kolo (UNESCO)
  - Museum of Broken Relationships
- **Lifestyle:**
  - Coffee culture (kava na špici)
  - Fjaka
  - Island and boat life
  - Konoba dining

### Inferences
- Several of these items are UNESCO intangible heritage and high-profile, so they probably have Wikidata items with hr labels. Dishes and local clubs are more likely to exist on hr.wikipedia and thus in Wikidata. Hyper-local venues won't.
- Croatian specifics affect the dimension mapping. Klapa and kolo are strongly Communal, and Sinjska alka is Communal plus heritage. The beach and sea lifestyle cluster may emerge as its own local factor.

### Gaps
- No SPARQL run, so there is no measured coverage of hr labels for subclasses of Q349 (sport), Q47728 (hobby), Q746549 (dish) [UNVERIFIED ID] or Q2095 (food) [UNVERIFIED ID].
- No Croatian national leisure participation survey (e.g. from the DZS statistics bureau) was consulted.
