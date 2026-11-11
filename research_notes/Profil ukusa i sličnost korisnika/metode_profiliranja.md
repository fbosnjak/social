# Computational methods for cross-domain user taste profiles and user-user similarity

> Researched 2026-11-11. About 20 search/fetch calls. Items marked "[background, unverified this session]" come from prior knowledge and were not re-checked against a source in this session. Treat them as leads, not citations.

## 1. Cross-domain recommendation (CDR): taxonomy, transfer methods, few overlapping users

### Takeaway
CDR methods fall into content-based transfer (link domains through shared features such as tags, text or knowledge-graph entities, with no shared users needed), embedding-based transfer (learn per-domain embeddings, then map them through overlapping users, as in EMCDR) and rating-pattern transfer. When few users overlap, mapping-based methods are fragile because the mapping is fit only on those users. Content and semantic bridges, or LLM-based shared semantic spaces, are the realistic option for a new events platform that starts with zero interaction history.

### Cited Findings
- Zhu et al. (IJCAI 2021) sort CDR into four scenarios: single-target, multi-domain, dual-target and multi-target. They define a "domain" by content-level relevance (shared features or tags, no shared users, e.g. Amazon Music vs Netflix), user-level relevance (shared users with different item types, e.g. movies vs books) and item-level relevance (shared items, different users) — [Zhu et al. 2021, arXiv 2103.01696](https://arxiv.org/abs/2103.01696)
- The same survey groups conventional approaches as (1) content-based transfer through item details, reviews, social tags and semantic properties (for content-level relevance), (2) embedding-based transfer: train CF models (SVD, PMF, BPR, NCF, DMF, graph models) per domain, then transfer embeddings through common users or items, and (3) rating-pattern transfer — [Zhu et al. 2021](https://arxiv.org/pdf/2103.01696)
- Named challenges: CH1 building content-based relations, CH2 generating accurate embeddings, CH3 learning accurate mapping relations (naive replacement of target embeddings with source ones is "simple but not intelligent"), CH4/CH5 dual-target design and embedding optimisation, CH6 negative transfer (adding sparser domains can *degrade* performance, which is worst in multi-target CDR) — [Zhu et al. 2021](https://arxiv.org/pdf/2103.01696)
- Prospects named by the survey: heterogeneous CDR (domains rich in different data types, e.g. interactions vs social graph), sequential CDR and privacy-preserving CDR — [Zhu et al. 2021](https://arxiv.org/pdf/2103.01696)
- EMCDR (Man et al., IJCAI 2017) has three steps: latent factor modelling per domain, a latent-space mapping function trained on overlapping users, and recommendation for cold-start users by mapping their source embedding into the target space — [Man et al. 2017, IJCAI](https://www.ijcai.org/proceedings/2017/0343.pdf)
- Known weakness: the mapping is learned only on limited overlapping users, so it can be biased toward them and generalise poorly to cold-start users. One study finds EMCDR converges to sharp minima. Follow-ups include Transfer-Meta (task-oriented meta network, arXiv 2105.04785), CDRIB (variational information bottleneck, jointly learned embeddings, arXiv 2203.16863) and SCDR (sharpness-aware optimisation, reported to "significantly outperform" other CDR models on Amazon cold-start tasks) — [CDRIB arXiv 2203.16863](https://arxiv.org/pdf/2203.16863); [Transfer-Meta arXiv 2105.04785](https://arxiv.org/abs/2105.04785v1); [SCDR arXiv 2408.01931](https://arxiv.org/pdf/2408.01931)
- Semantic / Linked-Data bridges: Fernández-Tobías, Cantador, Kaminskas and Ricci (HetRec@RecSys 2011) proposed a generic framework linking domains through DBpedia semantic networks. A later UMUAI paper (2019, s11257-018-9217-6) uses Linked Open Data features and relations as inter-domain links for user cold start and feeds those similarities into hybrid matrix factorisation — [UniBZ record, 2011 framework](https://bia.unibz.it/esploro/outputs/conferenceProceeding/A-generic-semantic-based-framework-for-cross-domain/991006493483801241); [UMUAI paper PDF](https://iris.poliba.it/bitstream/11589/203378/4/10.1007_s11257-018-9217-6.pdf); [SisInfLab summary](https://sisinflab.poliba.it/publications/addressing-the-user-cold-start-with-cross-domain-collaborative-filtering-exploiting-item-metadata-in-matrix-factorization/)
- Heitmann and Hayes's SemStim (2016) uses knowledge-graph (DBpedia) links to recommend in one domain (e.g. music) from preferences in another (e.g. movies) — [search summary pointing to UMUAI/thesis listings](https://repositorio.uam.es/bitstream/handle/10486/677710/fernandez_tobias_ignacio_javier.pdf?sequence=1) (secondary; primary SemStim paper not fetched)
- Industry evidence (Spotify, SIGIR 2020): transferring *music* listening representations to predict *podcast* preferences for cold-start users gave up to 50% improvement offline and online. The simpler aggregated-metadata representation performed about as well as latent embeddings — [Nazari et al., arXiv 2007.13287](https://arxiv.org/pdf/2007.13287)
- Spotify (2024) built a co-listening graph of audiobooks and podcasts (edge = at least one user streams both) with a GNN plus a two-tower model. A secondary summary reports +7% Hit-Rate@10 and +80% coverage for podcasts — [arXiv 2403.05185](https://arxiv.org/pdf/2403.05185.pdf) (figures from summary, not verified in the PDF)
- A Spotify patent family describes neural user embeddings learned from music consumption and used to pick second-domain content by nearest-neighbour search. A patent does not confirm deployment — [US11681739B2](https://patents.google.com/patent/US11681739)

### Inferences
- An events social network will have *content-level* relevance (shared semantic features across music, films, books and events) and almost no user-level overlap with any external system. Embedding-mapping methods (EMCDR family) need a set of users with data in both domains. They only become usable once the platform has collected many multi-domain profiles itself.
- The Spotify result (metadata-based ≈ latent embeddings for music→podcast) suggests that a well-chosen content representation can capture most of the cross-domain signal without heavy transfer learning.
- Negative transfer (CH6) argues for weighting domains per user, or gating them, rather than naively concatenating all domains.

### Gaps
- Khan et al.'s CDR survey (ACM CSUR 2017) was not retrieved in this session.
- I found no head-to-head benchmark of EMCDR-family vs content/KG bridges under *very* few overlapping users (<1%).

## 2. Content-based item representations: language models, LLM user profiles, knowledge graphs, audio

### Takeaway
Language-model and LLM representations can place items from different domains in one shared semantic space and can produce human-readable profiles. With little or no interaction data they are competitive with CF: zero-shot LLMs alone are weak at rating prediction, but in near cold-start, language-based preferences are competitive (Sanner et al. 2023). Fine-tuned or hybrid LLMs close the gap with CF. Audio embeddings handle music cold start. Knowledge graphs give explicit cross-domain links.

### Cited Findings
- Sanner, Balog, Radlinski, Wedin and Dixon (RecSys 2023) collected item-based and language-based preferences plus ratings on recommended and random items. Zero- and few-shot LLM prompting was **competitive with fully supervised item-based CF in near cold-start**, using either item-based or language-based preferences. Language-based preferences are more explainable and scrutable — [arXiv 2307.14225](https://arxiv.org/pdf/2307.14225); [Google Research page](https://research.google/pubs/large-language-models-are-competitive-near-cold-start-recommenders-for-language-and-item-based-preferences/)
- Kang et al. (Google, 2023; models from 250M to 540B parameters): **zero-shot LLMs lag CF models that have interaction data** and only match trivial baselines (global item or user mean). With fine-tuning they reach comparable or better performance using only a small fraction of the training data — [arXiv 2305.06474](https://arxiv.org/abs/2305.06474v1)
- Ramos et al. (ACL 2024) prompt LLMs to write a natural-language (NL) user profile, then fine-tune an LLM to recommend using only that NL profile. Accuracy was comparable to (not better than) baselines. The benefit is transparency and scrutability: editing the profile measurably shifts recommendations — [ACL Anthology 2024.acl-long.753](https://aclanthology.org/2024.acl-long.753.pdf); [arXiv 2402.05810](https://arxiv.org/abs/2402.05810v2)
- Related: Radlinski et al. 2022 (NL user profiles for scrutable recommendation) and Mysore et al. SIGIR 2023 (editable user profiles for controllable text recommendation) — cited inside [Ramos et al. 2024](https://aclanthology.org/2024.acl-long.753.pdf) (not fetched separately)
- LLM cross-domain work from 2025–2026: LLM4CDR (zero-shot, two-step prompting: first generate "recommendation guidance" such as shared genre or theme features to bridge domains, then predict); LLM4CDSR (SIGIR 2025, hierarchical LLM profiling module that summarises cross-domain preferences); LLM-RecG (arXiv 2501.19232, adapts user embeddings to unseen domains zero-shot); RecBench-MD (arXiv 2508.21354, benchmark of foundation models in zero-resource multi-domain settings); SPHERE (arXiv 2606.01783, structured "semantic personas" for CDR) — [LLM-RecG](https://arxiv.org/html/2501.19232v2); [RecBench-MD](https://arxiv.org/pdf/2508.21354v1); [SPHERE](https://arxiv.org/pdf/2606.01783); [LLM-EDT arXiv 2511.19931](https://arxiv.org/pdf/2511.19931); [arXiv 2504.18383](https://arxiv.org/pdf/2504.18383v1) (metrics not extracted; from search summaries only)
- Spotify GLIDE injects long-term user embeddings as soft prompts into a generative podcast recommender. It reports up to +5.4% non-habitual podcast streaming on the home surface — search summary of Spotify work, primary not fetched (see [Spotify results list](https://arxiv.org/pdf/2403.05185.pdf) for the neighbouring paper)
- Audio content: van den Oord, Dieleman and Schrauwen (NIPS 2013) predict CF latent factors from audio for cold-start songs on the Million Song Dataset. Deep CNNs significantly outperform bag-of-words audio features, and predicted factors give sensible recommendations despite the semantic gap — [NeurIPS paper](https://papers.nips.cc/paper/5004-deep-content-based-music-recommendation); [Dieleman blog on Spotify CNNs](https://sander.ai/2014/08/05/spotify-cnns.html)
- Knowledge graphs (DBpedia/LOD) as cross-domain bridges: see Section 1 (Fernández-Tobías et al. 2011, 2019) — [UMUAI 2019](https://iris.poliba.it/bitstream/11589/203378/4/10.1007_s11257-018-9217-6.pdf)

### Inferences
- A practical stack for heterogeneous favourites (songs, films, books): (a) embed each item from text, metadata, tags or a Wikipedia/Wikidata description with one shared text-embedding model, giving all domains one space; (b) optionally add domain-specific signals (audio embeddings for tracks); (c) generate an LLM NL summary of the profile for display and editing. Kang et al. and Sanner et al. together imply that LLM-only scoring should be hybridised with interaction data once it exists.
- NL profiles are a strong fit when "profile accuracy" includes *user-perceived* accuracy. Users can correct them, which is itself a feedback signal (Ramos et al.).
- Wikidata/DBpedia entity linking gives explicit cross-domain relations (a book adapted into a film, a soundtrack composer, shared genres or themes) that text embeddings capture only implicitly.

### Gaps
- No reported accuracy numbers were extracted for the 2025–2026 LLM-CDR papers. Abstracts were seen only through search summaries.
- I did not find studies evaluating general-purpose text-embedding models specifically for cross-domain *taste* similarity (music↔film↔book) against human judgements.
- Multimodal audio-text models (e.g. CLAP- or MERT-style) were not researched in this session [background, unverified].

## 3. Interpretable psychological dimensions vs latent factors

### Takeaway
Psychology offers validated low-dimensional taste spaces for music: MUSIC (5 style factors) and Greenberg et al.'s arousal/valence/depth (3 attribute dimensions). They are interpretable and link to personality, but effect sizes are small (multiple R about 0.2–0.26). They suit explanation and display layers better than replacing high-dimensional embeddings for matching.

### Cited Findings
- MUSIC model (Rentfrow, Goldberg and Levitin, JPSP 2011, 100(6):1139–1157): three studies converge on five genre-free latent factors: **M**ellow (e.g. pop/soft rock, R&B), **U**npretentious (e.g. country), **S**ophisticated (e.g. classical, jazz), **I**ntense (e.g. rock), **C**ontemporary (e.g. rap). Preferences are driven by both social and auditory characteristics — [Scientific American summary](https://www.scientificamerican.com/blog/guest-blog/anything-but-country-what-factor-analysis-reveals-about-our-tastes-for-tunes/); [APA Monitor 2011](https://www.apa.org/monitor/2011/05/music); [FORRT replication atlas entry](https://forrt.org/flora-replication-atlas/doi/10.1037/a0022406/)
- MUSIC structure replicated (Kosinski and colleagues, "The Song Remains the Same") — [Stanford GSB](https://www.gsb.stanford.edu/index%2Ephp/node/428616)
- Greenberg et al. (Social Psychological and Personality Science, 2016, "The Song Is You"): 76 judges rated 102 excerpts from 26 genres and subgenres on 38 perceived attributes, which reduce by PCA to **arousal** (intense, forceful, abrasive, thrilling), **valence** (fun, happy, lively vs depressing, sad) and **depth** (intelligent, sophisticated, inspiring, complex, poetic, deep, emotional, thoughtful vs "party music"). In Study 2, N = 9,454 participants each rated 50 excerpts — [Greenberg et al. 2016 PDF](https://cdn2.psychologytoday.com/assets/greenberg%20et%20al.%20(2016)%20-%20the%20song%20is%20you_print%20version.pdf)
- Effect sizes: personality added to demographics raised multiple R from .21→.26 (arousal), .20→.21 (valence) and .12→.19 (depth). Facet correlations are small, e.g. excitement-seeking with arousal preference r = .20. Openness correlates with liking music broadly at r = .13. The authors call these "small to modest" but stable given the N. Openness → depth and positive valence. Neuroticism → arousal and negative valence. Agreeableness and conscientiousness → low arousal — [Greenberg et al. 2016](https://cdn2.psychologytoday.com/assets/greenberg%20et%20al.%20(2016)%20-%20the%20song%20is%20you_print%20version.pdf)
- Attribute (AVD) and style (MUSIC) models each explain unique variance in preferences, and neither predicts demographics or personality better than the other — [Greenberg et al. 2016](https://cdn2.psychologytoday.com/assets/greenberg%20et%20al.%20(2016)%20-%20the%20song%20is%20you_print%20version.pdf)
- The authors suggest extending to sonic attributes (timbre, instrumentation), similar to Pandora's Music Genome Project coding — [Greenberg et al. 2016](https://cdn2.psychologytoday.com/assets/greenberg%20et%20al.%20(2016)%20-%20the%20song%20is%20you_print%20version.pdf)

### Inferences
- To score a *user* on AVD or MUSIC you need *item* scores on those dimensions. Options: (a) human-rated anchor items plus regression from audio or text embeddings to the dimensions; (b) LLM zero-shot rating of items on attribute adjectives. Both need validation against the published attribute lists.
- These dimensions are music-specific. No equally validated cross-domain (film/book) equivalent turned up in this session. Combining them across domains would be a design choice, not an established method.
- Recommended pattern: match users on high-dimensional embeddings (more information) and *explain* matches through interpretable dimensions such as "you both like deep, low-arousal music".

### Gaps
- No source found that reports a validated automatic predictor of MUSIC or AVD scores from audio or metadata, with accuracy figures.
- Film and book analogues (e.g. psychological dimensions of film preference) were not researched.

## 4. Similarity measures between users represented as sets of items

### Takeaway
Set-overlap measures (Jaccard, cosine on item vectors) fail when users share no exact items, which is the norm across sparse "favourites" lists. Embedding-based measures fix this: mean-embedding cosine is cheap but blurs multimodal tastes, and optimal transport / Earth Mover's (PMD) matches items semantically and handles zero overlap. Popularity discounting (IUF/IDF) is a well-established boost for overlap-based measures.

### Cited Findings
- Breese, Heckerman and Kadie (UAI 1998) is the classic empirical comparison of memory-based CF. Correlation (Pearson) methods were strong, especially with **inverse user frequency** (IUF, weight log(n/n_j), the item analogue of IDF that down-weights items liked by many users), **default voting** (impute values for unrated items, which helps with small overlaps) and **case amplification** (raise weights to a power to emphasise strong neighbours). A reanalysis found the IUF and amplification benefits additive — [Breese et al. arXiv 1301.7363](https://arxiv.org/pdf/1301.7363); [Northeastern CF handout](https://www.khoury.northeastern.edu/home/jaa/IS4200.12S/Handouts/collab_filter.pdf)
- Preference Mover's Distance (PMD; arXiv 1909.04239): treats each user as a probability distribution over rated items and computes Earth Mover's (Wasserstein) distance with an item-item ground distance. It **works for users with no co-rated items** (where cosine and Pearson fail), is a true metric, and reports better accuracy on sparse data — [PMD arXiv 1909.04239](https://alphaxiv.org/abs/1909.04239v2); [Papers with Code](https://astro.paperswithcode.com/paper/pmd-a-new-user-distance-for-recommender)
- WSFE (2023): training-free, model-agnostic optimal-transport encoder of user-wise similarity in embedding space, tested on 6 recommender models × 6 datasets — [arXiv 2305.04410](https://arxiv.org/pdf/2305.04410)
- Word Mover's Distance is the text version (EMD over word embeddings, a special case of Wasserstein). Sinkhorn-Knopp makes it tractable — [arXiv 2005.06727](https://arxiv.org/pdf/2005.06727); [CEUR Vol-2960](https://ceur-ws.org/Vol-2960/paper2.pdf)
- The Last.fm Tasteometer algorithm is **not public**. A forum guess (cosine over listening vectors) is speculation. Users reported "super" compatibility despite large genre differences, which suggests it is not a simple share of artists. The API (`tasteometer.compare`) returned a score plus a list of shared artists — [Straight Dope thread](https://boards.straightdope.com/t/algorithm-for-last-fm/611726); [liblastfm Tasteometer API docs](https://hackage.haskell.org/package/liblastfm-0.7.0/docs/Lastfm-Tasteometer.html)

### Inferences (comparison; items without a link are [background, unverified this session])
| Measure | Strengths | Weaknesses | Data needs |
|---|---|---|---|
| Jaccard on item sets | Trivial, interpretable ("you share 4 films") | 0 when no exact overlap; dominated by popular items; size-sensitive | Exact item IDs |
| Cosine / Pearson on user-item vectors (+IUF) | Classic, validated (Breese 1998); IUF reduces popularity effect | Needs co-rated items; ratings rarely exist for "favourites" | Interaction matrix |
| Cosine of mean (centroid) embeddings | Cheap, works with zero overlap, one vector per user per domain | Averaging collapses multimodal tastes (metal + jazz ≈ "medium"); popular-item and hub effects in embedding space | Item embeddings |
| Set-to-set: Chamfer / average best-match | Keeps individual items, handles multimodality, explainable (which items matched) | Asymmetric unless symmetrised; outlier-sensitive | Item embeddings, O(n·m) |
| Optimal transport / Wasserstein (PMD, WMD-style) | True metric, zero-overlap safe, explainable transport plan | Costlier (Sinkhorn approximations); needs a ground metric | Item embeddings or distances |
| Kernel MMD between embedding sets | Principled distribution distance (two-sample-test lineage, Gretton et al. JMLR 2012) | Kernel bandwidth choice; less explainable | Item embeddings |
| Topic-model (LDA) mixtures + JS/Hellinger | Interpretable topics; built-in smoothing for sparse users | Needs a text corpus per item; topic quality varies; older | Item text or tags |

- For multi-domain users, compute similarity **per domain** (when both users have that domain) and combine with weights that reflect evidence per domain. This avoids having to compare across domains directly (see Section 8).

### Gaps
- I found no published head-to-head of mean-embedding cosine vs OT vs Chamfer vs MMD on *user taste similarity* against human-judged ground truth.
- No authoritative description of how Last.fm, Spotify Blend or similar products compute their percentages was found.

## 5. Converting raw similarity to an interpretable percentage

### Takeaway
Raw cosine or OT scores are not probabilities, and their distributions vary by measure and domain. Two defensible routes: (1) **percentile calibration**, where "87%" means more similar than 87% of user pairs (no ground truth needed), and (2) **supervised calibration** (Platt / isotonic) against an observed outcome such as "pair co-attended", "connected" or "rated the match good". Platt suits small calibration sets; isotonic needs more data.

### Cited Findings
- Niculescu-Mizil and Caruana (ICML 2005) compare Platt scaling (fit a sigmoid on a held-out set) and isotonic regression (non-parametric, monotone). Platt works best when the distortion is sigmoid-shaped. Isotonic is more flexible but **overfits and does worse than Platt when calibration data are scarce** — [ICML 2005, mlanthology](https://mlanthology.org/icml/2005/niculescumizil2005icml-predicting); [Emergent Mind summary of related boosting paper](https://www.emergentmind.com/papers/1207.1403)
- Last.fm showed compatibility as ordinal labels ("low", "medium", "high", "very high", "super") alongside a list of shared artists. Users saw labels move over time as listening changed — [Last.fm shoutbox examples](https://www.last.fm/user/patchinprov/shoutbox); [Straight Dope thread](https://boards.straightdope.com/t/algorithm-for-last-fm/611726)

### Inferences
- Percentile approach: compute the empirical CDF of similarity over random user pairs, *per measure and per domain combination*, and report the percentile. It is stable and comparable across measures. Downside: it is relative. "90%" can still mean low absolute agreement in a heterogeneous population, and it drifts as the user base changes. Recompute on a schedule.
- Supervised approach: define a ground-truth event (e.g. two users both attending, or a mutual follow after meeting) and fit P(outcome | similarity, evidence size). This gives a real "match probability", but needs logged outcomes and is subject to exposure bias, since users only co-attend what they were shown.
- Show uncertainty: two sparse profiles should not get "95%". Shrink the score toward the population mean by evidence amount (see Section 7), or show a confidence band. Label tiers in the Last.fm style plus shared items aid interpretability.

### Gaps
- No public documentation found for Spotify Blend's "taste match %" or similar consumer compatibility scores.
- No academic study found that validates user-facing compatibility percentages against perceived compatibility.

## 6. Popularity bias and cold start

### Takeaway
Popular items create spurious similarity (everyone likes the same blockbuster) and hurt niche users most. Down-weight with IUF/IDF or inverse-popularity weighting. For cold start, use onboarding elicitation chosen by active-learning criteria and transfer from domains the user already supplied.

### Cited Findings
- Abdollahpouri, Mansoury, Burke and Mobasher: users less interested in popular items are affected most by algorithmic popularity bias and get the most miscalibrated recommendations. Algorithms with more popularity-bias amplification have more miscalibration. Unfairness is framed as differing miscalibration across user groups — [arXiv 2008.09273 (RecSys 2020 LBR)](https://arxiv.org/abs/2008.09273); [arXiv 1910.05755](https://arxiv.org/abs/1910.05755v2)
- IUF (Breese et al. 1998) down-weights items liked by many users and improves correlation-based CF — [arXiv 1301.7363](https://arxiv.org/pdf/1301.7363)
- Elahi, Ricci and Rubens (Computer Science Review 2016, 20:29–50) survey active learning for CF preference elicitation. Strategies are classified by personalisation (whether items shown differ per user) and hybridisation (single vs multiple criteria). The original motivation was efficient sign-up flows. The survey compares strategies and gives usage guidelines — [dblp](https://dblp.dagstuhl.de/rec/journals/csr/ElahiRR16.html); [UniBZ record](https://bia.unibz.it/esploro/outputs/journalArticle/A-survey-of-active-learning-in/991006479696201241)
- Audio-to-latent-factor prediction addresses *item* cold start for music (van den Oord et al. 2013) — [NeurIPS](https://papers.nips.cc/paper/5004-deep-content-based-music-recommendation)
- LLM language-based preferences are competitive in near cold start (Sanner et al. 2023) — [arXiv 2307.14225](https://arxiv.org/pdf/2307.14225)

### Inferences
- In a favourites-based profile, a single item's evidence ≈ its IDF: sharing an obscure album says more than sharing a top-10 film. Apply IDF weights both in overlap measures and as item weights (mass) in centroid or OT measures.
- Onboarding: ask for a few items per domain, chosen to maximise information (high-variance, polarising, mid-popularity items, per active-learning heuristics). Also allow free text ("describe your taste"), which Sanner et al. show is a usable signal.

### Gaps
- No quantitative numbers on how many onboarding items per domain are needed for a stable profile were found in this session.

## 7. Group / event profiles from members (event = aggregation of attendees)

### Takeaway
Group recommender research (Masthoff) gives the aggregation strategies: average, least misery, average-without-misery, multiplicative, Borda, Copeland, plurality. No strategy wins everywhere, so the choice depends on context. For an event profile built from attendees, a weighted average of member embeddings plus a dispersion measure (how homogeneous the crowd is) is the natural core. Small attendee counts call for shrinkage toward a prior such as the organiser's history or the category mean.

### Cited Findings
- Masthoff (2004 and later) catalogues the strategies: average, least misery (group score = minimum member score), average without misery (average after excluding items below a threshold for any member), multiplicative, plurality voting, Borda count, Copeland rule. An early empirical study favoured the **multiplicative** strategy — [Masthoff UMUAI 2006 PDF](https://abdn.elsevierpure.com/files/3998167/UMUAI06.pdf); [TU Wien group RS tutorial](https://www.ec.tuwien.ac.at/sites/ec.tuwien.ac.at/files/news-attachments/Tutorial%20GRSs.pdf)
- Strategy performance depends on context (group type, evaluation criteria). Arrow's theorem is invoked to argue no single best strategy, which motivates adaptive selection. PolyLens (permanent groups): least-misery recommendations were accepted by 77% of users. Multi-criteria evaluation (average satisfaction, minimised misery) is recommended — [Delic et al. UMAP 2017](https://www.ec.tuwien.ac.at/~dimitris/publications/UMAP17.pdf); [UMUAI 2023 s11257-023-09363-0](https://link.springer.com/article/10.1007/s11257-023-09363-0)

### Inferences
- Event profile = attendance-weighted centroid (or mixture) of attendee taste embeddings, plus organiser's past events and explicit event metadata. Keep the *distribution*, not just the mean, e.g. a few cluster centroids. Then a user can be matched to the sub-crowd they would fit, and OT or Chamfer (Section 4) apply directly between user and event.
- Small samples: empirical-Bayes / Bayesian shrinkage, profile_event = (n·mean_attendees + k·prior)/(n + k), where the prior is the organiser's historical profile or the category mean and k is tuned on held-out data. This is standard empirical-Bayes logic [background, unverified this session]. Down-weight attendees with thin profiles.
- Least-misery-style signals help for "who would *not* enjoy this event" warnings. Average or multiplicative suit "overall vibe".
- Attendance is not the same as taste (people attend with friends). Weight by repeat attendance, explicit ratings or dwell signals where possible.

### Gaps
- No literature found specifically on deriving *event or venue profiles from attendee taste profiles* with reported accuracy (closest: group RS, plus Spotify's co-listening graph idea in Section 1).
- Bayesian shrinkage for group profiles was not sourced in this session.

## 8. Handling users missing some domains (partial profiles)

### Takeaway
Treat each domain as an optional view: compare only on the domains both users have, weight by per-domain evidence, and either impute missing domains through cross-domain transfer (mapping or LLM inference) with lowered confidence, or just leave them out. Negative transfer warns against blindly imputing.

### Cited Findings
- Cross-domain transfer exists precisely to fill sparse or missing domains from richer ones (Douban example: recommend books from movie reviews). Mapping methods (EMCDR) infer target-domain embeddings for users who have only source-domain data — [Zhu et al. 2021](https://arxiv.org/pdf/2103.01696); [Man et al. 2017](https://www.ijcai.org/proceedings/2017/0343.pdf)
- Adding domains can hurt (negative transfer, CH6) — [Zhu et al. 2021](https://arxiv.org/pdf/2103.01696)
- Music history predicts podcast taste well enough to give up to +50% for cold-start podcast users, showing a missing domain can be partly inferred from another — [Spotify, arXiv 2007.13287](https://arxiv.org/pdf/2007.13287)
- LLM-based profiling can summarise cross-domain preferences into one profile (LLM4CDSR, SPHERE personas) — [SPHERE arXiv 2606.01783](https://arxiv.org/pdf/2606.01783)

### Inferences
- Overall similarity = Σ_d w_d · conf_d(u, v) · sim_d(u, v) / Σ_d w_d · conf_d(u, v), summed over the domains d both users have. conf_d grows with the number of items both users supplied in d. Report which domains drove the match.
- A shared semantic space (one embedding model for all item types) allows *cross-domain* comparison (user A's books vs user B's films). It is weaker evidence than same-domain comparison, so give it a lower weight.
- Imputed domains (from mapping or LLM inference) should carry lower confidence and be shown as "inferred", consistent with the scrutability argument of Ramos et al. 2024.

### Gaps
- I found no empirical study comparing "skip missing domain" vs "impute missing domain" for user-user similarity accuracy.
