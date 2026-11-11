# Validating taste/personality profiles and user-user similarity

> Tags used below: **[verified]** means the number was read from the primary text (PDF extracted locally) or confirmed in search results. **[background, unverified]** means it comes from the researcher's prior knowledge and was not re-checked in this session.

## 1. Ground truth options for taste/personality profiles

### Takeaway
No single gold standard exists. Use several criteria together, as the reference studies do: (a) a validated self-report questionnaire (Big Five; STOMP/MUSIC for music); (b) behavioural held-out prediction; (c) ratings from a friend or partner (self-other agreement); (d) test-retest stability. The reliability of the criterion sets the ceiling on achievable accuracy.

### Cited Findings
- Kosinski, Stillwell & Graepel (2013, PNAS) compared prediction accuracy directly with questionnaire test-retest reliability. Likes-based Openness reached r = 0.43, against a test-retest reliability for Openness of r = 0.50, so the prediction was "very close" to the ceiling. For the other traits, accuracy was roughly half the test-retest reliability. [verified, PDF] — [Kosinski et al. 2013 PNAS PDF](https://wiki.helsinki.fi/xwiki/bin/download/SocStats/Tilastotieteen%20jatkokurssi%20%28osa%201%20ja%20osa%202%29%2C%20kev%C3%A4t%202013/WebHome/Kosinskiym2013.pdf?rev=1.2); [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC3625324)
- Youyou, Kosinski & Stillwell (2015, PNAS) used self-other agreement as the yardstick. A Likes-based prediction correlated with the self-report at r = 0.56. Facebook friends who filled in a personality questionnaire about the participant reached r = 0.49. With more than 500 Likes, the computer peaked at r = 0.66. Reported sample: 86,220 (PMC abstract; the Stanford Daily says 82,220). The computer beat a work colleague after 10 Likes, a friend or roommate after 70, a family member after 150, and a spouse after 300. [verified via search snippets] — [PMC4313801](https://pmc.ncbi.nlm.nih.gov/articles/PMC4313801); [Stanford Daily](https://stanforddaily.com/2015/01/14/researchers-find-computers-are-better-predictors-of-personality-than-friends-family/); [Stanford GSB](https://www.gsb.stanford.edu/insights/michal-kosinski-computers-are-better-judges-your-personality-friends)
- The Youyou 2015 authors also noted that humans may judge some traits better, namely traits that leave no digital footprint or that depend on subtle cognition. — [Stanford News](https://news.stanford.edu/2015/01/12/personality-computer-knows-011215/)
- The Youyou 2015 paper also reports interjudge agreement and external validity (prediction of life outcomes such as substance use and political attitudes) as further validity criteria. Numbers from memory: interjudge agreement of about r = 0.62 for computer pairs vs about 0.38 for human pairs. [background, unverified; could not open the full text, PNAS returned 403]
- STOMP (Rentfrow & Gosling 2003, JPSP 84:1236–1256): 14 genres, 7-point scale, 4 dimensions (Reflective & Complex, Intense & Rebellious, Upbeat & Conventional, Energetic & Rhythmic). Rentfrow, Goldberg & Levitin (2011) found that 5 factors (MUSIC: Mellow, Unpretentious, Sophisticated, Intense, Contemporary) fit better. — [Gosling lab STOMP page](https://gosling.psy.utexas.edu/scales-weve-developed/short-test-of-music-preferences-stomp/); [SJDM DMIDI entry](https://dlab.sauder.ubc.ca/sjdm/dmidi/Short%20Test%20Of%20Music%20Preferences.html)
- STOMP stability: one retest study (N = 88, ages 20–58, 5-month interval) concluded that music preferences were "high and constant" in stability. The exact coefficients were not retrieved (conference/thesis source). — [Lawendowski, Univ. Jyväskylä](https://jyx.jyu.fi/jyx/Record/jyx_123456789_41620)
- Greenberg et al. (2022) found that the MUSIC five-factor structure is invariant across 53 countries and generalizes across assessment methods (genre ratings vs reactions to audio excerpts). This supports using MUSIC as a cross-cultural criterion measure. — [Cambridge](https://www.cam.ac.uk/stories/musical-preferences-unite-personalities-worldwide); [PDF](https://Www.Gwern.net/doc/psychology/personality/2022-greenberg.pdf)

### Inferences
- Report accuracy as a fraction of the criterion's reliability ceiling, as Kosinski 2013 did. A correlation of r = 0.40 against a questionnaire with test-retest r = 0.50 is close to the maximum, not a "mediocre" result.
- For a taste profile, the most natural ground truth is behavioural held-out prediction: hide part of the user's items, or a whole domain, and predict them. Questionnaires (STOMP/MUSIC, Big Five) are a convergent-validity check. They should not be the only criterion, because the product claims "taste", not "personality".
- Self-other agreement fits a social product well. Pairs of friends can rate each other (using STOMP/MUSIC or the product's own taste dimensions), and the computed profile can be compared with both the self-report and the friend's report.
- Test-retest of the computed profile: build the profile from data at T1 and T2 (for example 4–8 weeks apart, or from two disjoint subsamples of the same history, i.e. split-half). Then measure profile stability with ICC or a correlation between the vectors. If the computed profile is less stable than the questionnaire, it is measuring noise.

### Gaps
- The exact test-retest coefficients for STOMP (Rentfrow & Gosling 2003) and MUSIC were not retrieved.
- The detailed numbers from Youyou 2015 (judge-specific accuracy, interjudge agreement, external validity) could not be read from the primary text (403). Common ICC interpretation thresholds (Koo & Li 2016: <.50 poor, .50–.75 moderate, .75–.90 good, >.90 excellent) are background knowledge and were not verified.

## 2. How Kosinski 2013, Youyou 2015, Nave 2018 and Greenberg 2022 validated their predictions

### Takeaway
Shared protocol: a large sample, a sparse user×item matrix, dimensionality reduction or a regularized linear model, and k-fold out-of-sample prediction (10-fold, nested for hyperparameters). Metrics were Pearson r for continuous traits and AUC for binary ones, compared against a demographics-only baseline and the reliability ceiling. Typical r for Big Five predictions from taste data is 0.15–0.45. Above about 0.40 is considered strong, and r ≈ 0.25–0.30 for Openness from music alone is the realistic level.

### Cited Findings
**Kosinski, Stillwell & Graepel 2013** [verified, PDF]:
- N = 58,466 US users, 55,814 unique Likes, on average 170 Likes per user (median 68). The user×Like matrix was reduced with SVD to k = 100 components (28% of the variance). These fed linear regression (continuous traits) or logistic regression (binary traits). Validation used 10-fold cross-validation. — [PDF](https://wiki.helsinki.fi/xwiki/bin/download/SocStats/Tilastotieteen%20jatkokurssi%20%28osa%201%20ja%20osa%202%29%2C%20kev%C3%A4t%202013/WebHome/Kosinskiym2013.pdf?rev=1.2)
- Results: age r = 0.75; network density r = 0.52; network size r = 0.47; Openness r = 0.43; Extraversion r = 0.40; Intelligence r = 0.39; other traits and satisfaction with life r = 0.17–0.30. For binary variables, AUC: homosexual vs heterosexual men 0.88, African American vs Caucasian 0.95, Democrat vs Republican 0.85. AUC = 0.50 is chance. — [PDF](https://wiki.helsinki.fi/xwiki/bin/download/SocStats/Tilastotieteen%20jatkokurssi%20%28osa%201%20ja%20osa%202%29%2C%20kev%C3%A4t%202013/WebHome/Kosinskiym2013.pdf?rev=1.2)
- Choice of the number of components: going from few to more SVD components raised Openness accuracy from r = 0.1 to r = 0.4 (Fig. S3). — same PDF
- Accuracy grows with the number of Likes available per user, i.e. a learning curve by amount of data. — same PDF

**Youyou et al. 2015**: see section 1. Computer 0.56 vs friend 0.49, peak 0.66 at >500 Likes, and Like thresholds for beating different human judges. — [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC4313801)

**Nave et al. 2018 (Psychological Science 29(7):1145–1158)** [verified, PDF]:
- Study 1: N = 22,252 rated their liking of unfamiliar music excerpts (myPersonality, 153 countries). Study 2: N = 21,929 Facebook Likes of music artists. — [Gwern PDF](https://Www.Gwern.net/doc/psychology/personality/conscientiousness/2018-nave.pdf); [PubMed](https://pubmed.ncbi.nlm.nih.gov/29587129/)
- Method: nested cross-validation with a 10-way split. Linear regression with a LASSO penalty, λ tuned by an inner 10-fold CV.
- Study 1 (Mix_A, n = 17,904), out-of-sample r of predicted vs self-reported traits: O = .25 [.23, .26]; C = .11; E = .16; A = .15; N = .12.
- Baselines: gender + age alone gave O = .03, C = .13, E = −.01, A = .06, N = .15. Music + gender + age (n = 8,100) gave O = .25, C = .16, E = .17, A = .18, N = .19.
- A "general baseline" (mean liking M + general liking factor g + demographics) separated specific taste from a general tendency to like music. The gain from specific taste was largest for Openness (Δr = .09, +55%) and Extraversion (Δr = .08, +79%), and small for N (.03), A (.02) and C (.01).
- Replication: a second survey (Mix_B), rock and jazz surveys, a non-US subsample (n = 1,596) and an over-30 subsample (n = 1,528). Differences between models were tested with Steiger's z.
- Study 2 (music Likes, N = 21,929): O = .30 [.29, .31]; C = .19; E = .21; A = .17; N = .18. For O and N this was roughly comparable to a work colleague's accuracy (benchmark from Youyou 2015). For the other traits it was 41–66% of a colleague's accuracy (Study 1 comparison).
- Bivariate effects are small. For example, Openness × Sophisticated music r = .16, and Agreeableness × general liking r = .14.

**Greenberg et al. 2022 (JPSP, doi 10.1037/pspp0000397)**:
- Study 1: N = 284,935 across 53 countries (genre ratings). Study 2: N = 71,714 across 36 countries (reactions to audio). The five-factor preference structure was invariant across countries (measurement invariance). Personality-preference correlations were consistent across countries and methods. For example, E goes with Contemporary and O with Sophisticated. Limitation: the sample was non-representative, US-dominated and focused on Western music. — [Cambridge](https://www.cam.ac.uk/stories/musical-preferences-unite-personalities-worldwide); [PDF](https://Www.Gwern.net/doc/psychology/personality/2022-greenberg.pdf)

### Inferences
- The minimum validation protocol for the prototype follows from these studies: (1) out-of-sample (k-fold) evaluation, never in-sample; (2) report r with a 95% CI per dimension; (3) compare against a demographics-only baseline and a "general activity" baseline (number of items, mean rating, popularity of consumed items); (4) a learning curve (accuracy vs number of connected items/sources); (5) the reliability ceiling.
- Realistic expectations: music alone → Openness r ≈ 0.25–0.30, other traits ≈ 0.10–0.20. Generic digital traces with many items → 0.40–0.56. If the prototype claims "92% similarity" while the underlying signal correlates with the criterion at r ≈ 0.3, the displayed number overstates precision. Consider showing uncertainty or coarse categories.
- A key Nave 2018 lesson: some predictive power comes from a general tendency (liking a lot, being active) rather than specific taste. The prototype should control for this, for example by normalizing ratings per user, so that "similarity" is not just "both are active".

### Gaps
- Greenberg 2022 country-level effect sizes were not extracted (full PDF not parsed).

## 3. Evaluating user-user similarity: computed vs perceived similarity, homophily outcomes, explanations

### Takeaway
Computed similarity is often a weak proxy for what people perceive as "similar" unless it is validated against human judgments. Validate the score along three lines: (a) agreement with users' own ratings of perceived similarity; (b) predictive validity for real social outcomes (connection formed, co-attendance at events, message replies); (c) user studies of explanations ("why are we 78% similar").

### Cited Findings
- Trattner & Jannach (UMUAI) trained models to predict human-perceived similarity (of movies). The combined models with the best offline performance also produced the highest user-perceived similarity. The authors stress that this check is often missing, because "we cannot always assume that a correspondence between offline results and user perception exists". — [UMUAI 2019 PDF](https://web-ainf.aau.at/pub/jannach/files/Journal_UMUAI_2019-2.pdf); [dars.uib.no](https://dars.uib.no/pubs/UMUAI2019.pdf)
- Comparison of item-item similarity algorithms with user ratings: the TMDb list was perceived as most similar, followed by cosine and TF-IDF. Pearson-based CF was "practically unusable" for users. Small sample, so indicative only. — [Zhang et al. PDF](https://s2.smu.edu/~jiazhang/Papers/JiaZhang-Movies.pdf)
- Jesse, Bauer & Jannach (UMUAI 2023, >500 participants): intra-list similarity (ILS, built on computed similarity) is a valid proxy for perceived diversity only for some implementations. Implementation details matter. — [UMUAI 2022/23 PDF](https://web-ainf.aau.at/pub/jannach/files/Journal_UMUAI_2022_Diversity.pdf); [Springer](https://link.springer.com/10.1007/s11257-022-09351-w)
- Homophily by taste, Lewis, Gonzalez & Kaufman (PNAS 2012): a college cohort on Facebook over 4 years (2006–2009). Students who shared certain music and film tastes were significantly more likely to become friends. Book tastes showed no such link. Peer influence (spread of taste) was rare, except for classical/jazz. — [Harvard Gazette](https://news.harvard.edu/gazette/story/2012/01/taste-test/); [phys.org](https://phys.org/news/2012-01-facebook-patterns-music-movies-friends.pdf)

### Inferences
- Design for validating the similarity score (pilot):
  1. **Perceived-vs-computed**: each participant sees N profiles (for example 10–20) spread across the range of computed similarity and rates "how similar is this person's taste to mine" (1–7). Analysis: within-person Spearman correlation, then the mean across people; or a mixed model with random intercepts per rater. Also check calibration: do people at a computed 80% receive higher ratings than people at 60%? Use binning and a calibration curve.
  2. **Predictive validity (homophily outcomes)**: logistic regression or AUC for whether a pair connects, sends a message, gets a reply, or attends the same event, as a function of computed similarity. Control for popularity, activity, geography and demographics. Lewis 2012 suggests the signal varies by domain: music and film count more than books for friendship. Weight domains accordingly and test that empirically.
  3. **Discrimination test**: real friend pairs vs random pairs → AUC of the similarity score. Friend pairs should be more similar, but there is a confound (friends influence each other), so treat this as weak evidence.
  4. **Event profile**: hold out one attendee. Does the event profile (aggregated from the other attendees) rank that person higher than non-attendees? Use AUC / precision@k.
- "% similarity" is a calibration claim. If the system says 80%, users should perceive it as clearly stronger than 50%. Without calibration, show percentiles relative to the population ("more similar than 90% of users") instead.

### Gaps
- No study was found that directly validates "% taste similarity" between people (person-person, not item-item) against perceived similarity. The literature found covers item-item similarity. This is a genuine gap, and it is the prototype's own research question.
- Classic explanation studies (Herlocker, Konstan & Riedl 2000; Tintarev & Masthoff) were not retrieved in this session. [background, unverified: Herlocker 2000 found histograms of neighbours' ratings to be the most persuasive explanation type]

## 4. Offline recommender metrics, leave-one-domain-out, and pitfalls

### Takeaway
Offline metrics (precision/recall@k, NDCG, AUC, coverage, novelty, diversity, popularity bias) are an imperfect proxy for online performance. Use them for fast comparison of variants, not as proof of profile accuracy. For cross-domain taste, the key test is leave-one-domain-out: predict books from music + film, and so on.

### Cited Findings
- Krauth et al. (2020): in simulation, offline metrics correlated with online performance across a range of environments, but improvements in offline metrics gave diminishing returns online. — [arXiv 2011.07931](https://arxiv.org/pdf/2011.07931)
- Peska & Vojtas: in small e-commerce, 800 algorithm variants were compared offline on 18 metrics against online A/B results. The offline-online relationship is not straightforward. — [arXiv 1809.03186](https://arxiv.org/pdf/1809.03186)
- Hidasi & Czapp: widespread flaws in offline evaluation. Offline evaluation is "just an imperfect proxy" of online performance but will remain the main method in research. — [arXiv 2307.14951](https://arxiv.org/pdf/2307.14951)
- Garcin et al. (news): a large gap between offline and online accuracy (reported via a secondary source). Maksai, Garcin & Faltings (RecSys 2015) combine accuracy, diversity, coverage and serendipity to predict online CTR. — [arXiv 1809.03186 (related work)](https://arxiv.org/pdf/1809.03186); [review](https://liner.com/review/predicting-online-performance-news-recommender-systems-through-richer-evaluation-metrics)
- Cross-domain on Douban: movie, book and music domains are linked by a shared user ID, typically with a ≥5-interaction filter. Overlap examples: 1,815 users shared between movie and music, and 2,209 between movie and book. — [arXiv 2407.12374](https://arxiv.org/pdf/2407.12374); [arXiv 2301.11467](https://arxiv.org/pdf/2301.11467)

### Inferences
- Metric set for the prototype: ranking quality (NDCG@10, Recall@k, AUC); calibration (does the predicted genre distribution match the held-out one, e.g. via KL divergence); coverage; popularity bias (how far the profile is just "the mainstream"); and diversity/novelty (ILS, with the Jesse et al. 2023 caveat about implementation).
- Pitfalls to guard against:
  - Popularity-based baseline: always compare with "recommend the most popular items" and "similarity = overlap of popular artists". A taste profile has to beat it.
  - Temporal leakage: split by time rather than at random, so the future is predicted from the past.
  - Sampled metrics: evaluating against a small sample of negatives inflates results. [background, unverified: Krichene & Rendle 2020]
  - Non-random missingness: users rate what they chose, so hold-out sets are biased toward familiar items.
  - Leave-one-domain-out: build the profile from domains A and B, then measure prediction of domain C. Compare with (i) a profile from domain C alone using its own CV, as the upper bound, and (ii) a popularity baseline for C. This directly tests whether the profile captures cross-domain taste rather than domain-specific listening.
- An offline result is necessary but not sufficient. The final validation of "profile accuracy" must include human judgment (sections 3 and 5).

### Gaps
- Primary sources for metric definitions (NDCG: Järvelin & Kekäläinen 2002; calibration: Steck 2018; popularity bias: Abdollahpouri) were not retrieved in this session. [background, unverified]

## 5. Online evaluation: A/B tests, interleaving, ResQue, trust and explainability

### Takeaway
For user-perceived quality, the standard is ResQue (Pu, Chen & Hu, RecSys 2011): a questionnaire with 13 constructs and 60 items, rooted in TAM, covering perceived accuracy, explanation, trust, satisfaction and intention to return. A/B tests and interleaving need traffic the prototype will not have, so with a small pilot a within-subject user study with ResQue items makes more sense.

### Cited Findings
- ResQue: 13 constructs, 60 items. It measures perceived system qualities (accuracy, novelty, diversity, interface), user beliefs (usefulness, ease of use, transparency/explanation), attitudes (satisfaction, trust/confidence) and behavioural intentions (use, purchase, return). Partly inspired by the Technology Acceptance Model. RecSys 2011, pp. 157–164, doi 10.1145/2043932.2043962. — [PDF](https://www.comp.hkbu.edu.hk/~lichen/download/p157-pu.pdf); [Bibsonomy](https://Bibsonomy.org/bibtex/232cc8cb5052fc4dd369c474a8a4d2853/jaeschke); [arXiv primer](https://arxiv.org/pdf/2302.02579)

### Inferences
- In the pilot, add ResQue items (adapted: "the similarity score matches my impression", "I understand why I am similar to this person", "I trust the taste profile") to a short post-session questionnaire.
- Online A/B test (later): compare variants of the similarity algorithm on behaviour (connection requests, message replies, event attendance). Interleaving (mixing two rankings in one list, e.g. team-draft) needs far fewer users than an A/B test for ranking comparisons. [background, unverified: Chapelle et al. 2012 report roughly 10–100x greater sensitivity]
- Explainability: show which shared artists, films or books drive the similarity score. This doubles as a validation tool, because users can flag wrong reasons ("I don't actually like that").

### Gaps
- The full ResQue item list and its psychometric results (Cronbach's α, SEM) were not extracted from the PDF. Read the PDF before building the questionnaire. A Knijnenburg et al. 2012 (UMUAI) user-experience framework alternative was not checked.

## 6. Public datasets for prototyping

### Takeaway
No large public dataset links the same users across music, film and books together with personality. The closest options: Douban (the same users on movie + book + music, no personality data), Amazon multi-category reviews, and Last.fm datasets (demographics only). myPersonality is no longer shared, and LFM-2b is no longer downloadable.

### Cited Findings
- **myPersonality**: the authors stopped sharing the data in April 2018 because of the workload of vetting and regulatory compliance. Facebook suspended the app on 7 April 2018. A credential leak on GitHub exposed data on >3 million users for 4 years (New Scientist). — [myPersonality site](https://sites.google.com/michalkosinski.com/mypersonality); [Gizmodo](https://gizmodo.com/the-other-cambridge-personality-test-has-its-own-databa-1823997062); [InfoDocket](https://www.infodocket.com/2018/05/14/report-huge-new-facebook-data-leak-exposed-intimate-details-of-3-million-users/)
- **LFM-2b** (Schedl et al., CHIIR 2022): >120k Last.fm users, >2 billion listening events (Feb 2005 – Mar 2020), 50M tracks, 5M artists. Demographics: country, age, gender. Spotify URIs for 2.38M tracks, tags for 2.23M. **No longer downloadable due to licence issues.** — [LFM-2b page](https://www.cp.jku.at/datasets/LFM-2b/); [CHIIR paper](https://humrec.github.io/publication/schedl-chiir-2022/schedl-chiir-2022.pdf)
- **Douban cross-domain**: movie, book and music with a shared user ID. Sizes depend on preprocessing. One version has 2,712 movie users, 1,820 music users and 2,212 book users, with 1,815 movie∩music and 2,209 movie∩book. An older crawl had about 11k users with ~1.4M movie, ~1.2M music and ~580k book ratings. There is no official release page. — [arXiv 2407.12374](https://arxiv.org/pdf/2407.12374); [arXiv 1210.7056](https://arxiv.org/pdf/1210.7056); [arXiv 2407.00909](https://arxiv.org/pdf/2407.00909)
- **PANDORA** (Gjurković et al., SocialNLP 2021): >10k Reddit users, >17M comments. Big Five labels for ~1.6k users, plus MBTI/Enneagram, age, gender and location. Text only, no taste data. — [ACL Anthology](https://aclanthology.org/2021.socialnlp-1.12); [arXiv 2004.04460](https://alphaxiv.org/abs/2004.04460)
- **Nave 2018** has the open-data/open-materials badges (keywords "open data, open materials"), so the excerpt data and some of the personality data may be available through OSF. — [PubMed](https://pubmed.ncbi.nlm.nih.gov/29587129/); [Caltech](https://resolver.caltech.edu/CaltechAUTHORS:20180402-080900774)

### Inferences
- Recommended combination: Douban for the leave-one-domain-out test (the same users across music, film and books, which matches the product's case exactly). MovieLens (film, with tags), the UCSD Goodreads Book Graph (books, with shelves), the Spotify Million Playlist Dataset (music, no users' personality) and Amazon Reviews (multi-category, shared reviewer ID) serve as single-domain benchmarks. For the link to personality, use Nave 2018 open data (if available on OSF) or PANDORA. [The sizes and licences of MovieLens, Goodreads, MPD and Amazon were not verified in this session.]
- None of these datasets contains perceived similarity between users or event attendance. That ground truth has to be collected in your own pilot.

### Gaps
- Not verified: current availability and licences of the UCSD Book Graph, the Amazon Reviews 2023 release, the Spotify MPD (AIcrowd challenge terms), LFM-1b, and the Ferwerda/Schedl Last.fm+personality datasets.
- No public dataset with event attendance plus taste profiles was found.

## 7. Synthetic personas vs real data: circular validation

### Takeaway
If personas are generated from the same model of taste (or the same feature space) that the algorithm uses, the algorithm will "recover" them almost perfectly. That only proves internal consistency, not accuracy about real people.

### Cited Findings
- Hidasi & Czapp warn that offline evaluation is an imperfect proxy even on real data, and synthetic data adds another layer of assumptions. — [arXiv 2307.14951](https://arxiv.org/pdf/2307.14951)
- Krauth et al. use simulation precisely to study offline-online agreement, but conclude that the result depends on the simulator's assumptions (correlation across "a range of environments", with diminishing returns). — [arXiv 2011.07931](https://arxiv.org/pdf/2011.07931)

### Inferences
- Synthetic personas are legitimate for: pipeline tests (bugs, edge cases such as one source only or 3 items), checks that similarity is symmetric and robust to adding noise, and sanity checks (identical persona → 100%, disjoint → low).
- Circularity is a risk when: (a) the persona generator uses the same genre/MUSIC dimensions as the profile model; (b) an LLM generates both the personas and their "true" preferences; (c) success is measured on the same features the persona was generated from. Mitigation: generate personas from real users' data (e.g. Douban/MovieLens profiles) and compare only against held-out behaviour. Never present synthetic recovery as evidence of profile accuracy.

### Gaps
- No dedicated primary source on "circular validation with synthetic personas" in recommender systems was found. The inferences above are methodological reasoning.

## 8. Sample sizes for a meaningful pilot

### Takeaway
Detecting a correlation of r = 0.30 (α = .05 two-tailed, power .80) needs about 85 participants, and r = 0.20 needs about 194. For pairwise analyses, the number of pairs grows quadratically, but pairs are not independent, so mixed models or permutation tests are required.

### Cited Findings
- The reference studies used samples of 17,904–284,935 for stable estimates of small effects (r ≈ .1–.3). Even there, Nave's smallest subsample (Jazz_B, n = 887) detected only Openness reliably. — [Nave 2018 PDF](https://Www.Gwern.net/doc/psychology/personality/conscientiousness/2018-nave.pdf)
- The STOMP retest study used N = 88. — [Lawendowski](https://jyx.jyu.fi/jyx/Record/jyx_123456789_41620)
- In Jesse et al. (perceived vs computed diversity), >500 participants were needed to compare several metric implementations. — [UMUAI](https://link.springer.com/10.1007/s11257-022-09351-w)

### Inferences
- Computed power (Fisher z approximation, α = .05 two-tailed, power .80): r = .10 → n ≈ 783; r = .20 → n ≈ 194; r = .30 → n ≈ 85; r = .40 → n ≈ 47; r = .50 → n ≈ 29.
- Suggested pilot design:
  - **Phase A (n ≈ 30–50)**: qualitative and feasibility work. Do people understand the profile? Do they recognise themselves ("face validity", 1–7 rating)? Test-retest of the profile. Detects only large effects (r ≥ .45).
  - **Phase B (n ≈ 100–200)**: convergent validity with STOMP/MUSIC and Big Five (target r ≥ .30 for the matching dimensions). Perceived-vs-computed similarity: each person rates 10–20 others, giving 1,000–4,000 judgments. Analyse with a mixed model (random effects for rater and target), since this design has far more power than n alone suggests.
  - **Friend pairs (≥50–100 pairs)** for self-other agreement.
  - Pre-register hypotheses and thresholds, e.g. "computed similarity correlates with perceived similarity at r ≥ .30" and "AUC for co-attendance ≥ .65". [The thresholds are suggestions, not from the literature.]

### Gaps
- No primary source was found with a recommended sample size specifically for pilot studies of perceived similarity in recommender systems. The numbers above are computed power, not literature norms.
