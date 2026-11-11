# Comparable products: how taste profiles, taste compatibility and event/people matching work

> Research date: 2026-11-11. About 25 search/fetch calls. Many product algorithms are not public; confirmed facts are kept separate from speculation below.

## 1. Spotify: Blend, taste profiles, Wrapped listening personality, user-embedding research

### Takeaway
Spotify has never published how the Blend "taste match" % is computed. The engineering blog only describes how Blend picks tracks, using four quality attributes and a "maximize the joy" rule. Wrapped's Listening Personality is a transparent 4-binary-axis typology (16 types, MBTI-style) about *how* you listen rather than *what* you listen to. Underneath, Spotify's research uses learned user embeddings: word2vec on playlists, session-level RNNs, and a general-purpose autoencoder user embedding (2025).

### Cited Findings
- Blend (launched 2021) lets any two users generate a shared playlist. It grew out of Family Mix and Duo Mix, which were limited to Family/Duo plan members — [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Blend's four quality attributes: **Relevant** (each track reflects that user's taste), **Coherent** (the playlist flows), **Equal** (both users are equally represented), **Democratic** (tracks both like rise to the top) — [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Key product decision: "maximize the joy" over "minimize the misery", so relevance to each individual beats group consensus. Each track carries the profile picture of the user it is attributed to, so attribution must be truthful — [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Blend explicitly has to handle pairs with little overlap (low-overlap pairs make strong Blends harder). Quality was evaluated with offline metrics plus a human "Content Recommendation Scorecard" from the Data Curation team. One coherence change was dogfooded internally for about a month before rollout — [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Blend "Data Stories" show a **taste match score** and "the artist that brings you together". Wrapped Blend shows top mutual artists and the top mutual genre of the year. The engineering post does not explain how the score is calculated — [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Press at launch: the score is shown as a percentage along with the song that "brings you together" — [Android Central](https://www.androidcentral.com/spotify-blend-now-official-letting-friends-compare-their-musical-taste); [TechSpot](https://www.techspot.com/news/91043-spotify-rolls-out-blend-playlists-combines-listening-tastes.html)
- Blend was expanded to groups of up to 10 people and to Blends with select artists (2022) — [TechCrunch](https://techcrunch.com/2022/03/30/spotify-expands-its-music-matching-playlist-blend-to-work-with-up-to-10-people-including-select-artists); [TechCrunch](https://techcrunch.com/2022/07/20/spotify-adds-more-artists-blend-tool/amp/)
- User complaints: match percentages that dropped suddenly, and reports that the two sides see different numbers (one forum report claims 4% on one side and 88% on the other). This points to an asymmetric or unstable metric, but Spotify has not confirmed it — [Spotify Community](https://community.spotify.com/t5/Content-Questions/My-taste-match-percentages-in-my-blends-recently-dropped/m-p/6281660)
- Wrapped 2022 "Listening Personality": a combination of **four binary attributes**, giving 16 types. Spotify says the types are "independent of what music you like" and lets the 16 types occur in natural proportions. Axes: Familiarity vs Exploration, Loyalty vs Variety, Timelessness vs Newness, Commonality vs Uniqueness. Spotify frames the result as "only a description of how you've been listening, recently" — [Spotify Engineering: What's a Listening Personality?](https://engineering.atspotify.com/2023/1/whats-a-listening-personality); [Denver7](https://www.denver7.com/spotifys-wrapped-2022-lets-you-hear-your-music-listening-personality-for-this-year)
- Press sources disagree on some code-to-name mappings for the types, e.g. Specialist vs Devotee — [Yahoo](https://www.yahoo.com/lifestyle/16-personality-types-spotify-wrapped-195652285.html); [Says](https://says.com/my/amp/entertainment/spotify-wrapped-2022-16-personalities)
- Spotify research: user and track vectors computed daily by running word2vec on user playlists (playlists = documents, tracks = words) — [arXiv 2401.08818, "Link Me Baby One More Time: Social Music Discovery on Spotify"](https://arxiv.org/pdf/2401.08818)
- CoSeRNN: a user is represented as a *sequence of session embeddings*, with preferences modeled at the start of each session using context (time, device). There is also a matching patent — [KU research profile](https://researchprofiles.ku.dk/da/publications/contextual-and-sequential-user-embeddings-for-large-scale-music-r/); [US patent 11782968](https://patents.justia.com/patent/11782968)
- Generalized user representations: an autoencoder compresses multi-signal features into compact general-purpose user embeddings, which downstream products reuse via light transfer learning (Spotify Research, 2025) — [arXiv 2403.00584](https://arxiv.org/pdf/2403.00584v1.pdf); [Spotify Research post (mirror)](https://scour.ing/p/https://research.atspotify.com/2025/9/generalized-user-representations-for-large-scale-recommendations)
- Third-party reconstruction of a better Blend track scorer: drop tracks one user never played and take the *minimum* per-user score, so a track one person loves and the other doesn't know cannot win. This is a developer's proposal, not Spotify's method — [holewinski.dev](https://www.holewinski.dev/writing/fixing-spotify-blends)

### Inferences
- Spotify treats the shared percentage as an emotional hook (paired with "the artist that brings you together"), not a precise metric. The lack of a public formula and user reports of instability suggest it comes from embedding similarity, which can move when the models are retrained. This is speculation.
- The Listening Personality design is a useful pattern. It describes *behaviour* (exploration, loyalty, recency, mainstreamness), which works across music, film and books, and it avoids genre labels.
- Spotify's split between "maximize joy for each person" and "democratic overlap" applies directly to event recommendations based on who attends.

### Gaps
- No official source gives the Blend taste-match formula, whether it is symmetric, or which inputs it uses (time window, artists vs tracks vs genres).
- I did not check the current status (2025–2026) of Blend features such as Wrapped Blend or artist Blends.

## 2. Last.fm Tasteometer / compatibility

### Takeaway
Last.fm's "Tasteometer" (a compatibility meter between two users or artist lists) never had a published formula. The public API `tasteometer.compare` returned a score plus a list of shared artists, and the UI showed a coloured bar with a word label and a few shared artists. Community guesses describe a cosine similarity on user-by-artist play vectors.

### Cited Findings
- The API's `tasteometer.compare` took two inputs (users or artist lists) and returned a score plus shared artists — [liblastfm Hackage docs](https://hackage.haskell.org/package/liblastfm-0.7.0/docs/Lastfm-Tasteometer.html)
- Users describe the on-site meter as a colour-graded bar with a handful of shared bands listed below it. People asking how it worked got no official answer, and the commonly offered guess was the cosine between user-by-artist vectors. That guess is speculation — [Straight Dope forum](https://boards.straightdope.com/t/algorithm-for-last-fm/611726)
- Academic research on online music profiles (2013) found that **similar motivations for listening and perceived individuality** predicted interpersonal attraction more strongly than shared genres or artists — [Springer chapter](https://link.springer.com/chapter/10.1007/978-3-642-40477-1_46)

### Inferences
- Last.fm's design (a qualitative band plus a concrete list of shared artists) is a precedent for showing *why* two people match rather than only a number.
- The 2013 finding supports including "how you consume" dimensions, as in Spotify's Listening Personality, alongside "what you like".

### Gaps
- No confirmed formula. Not verified: the exact label tiers (from memory, labels like "Very Low… Super"), and the date the Tasteometer was retired. The API method is widely believed to be deprecated, but I found no primary source for this.

## 3. Dating and friend apps that use taste or quizzes (OkCupid, Hinge, Tinder, Bumble BFF, Lex, Pie, Timeleft, 222, IRL)

### Takeaway
OkCupid's question-based match % is the best-documented public model of a "% compatibility" number. It uses importance-weighted satisfaction in both directions, combined by a geometric mean, so a mismatch in either direction is punished. Dating apps (Tinder, Hinge) use Spotify mostly as *profile decoration* (an anthem or top artists), not as a scored match. New IRL-group apps (Timeleft, 222, Pie) use a short personality quiz plus logistics constraints to form small groups and do not publish formulas.

### Cited Findings
**OkCupid**
- Each question has your answer, the answers you'd accept, and an importance rating. Importance weights are 0 (irrelevant), 1 (a little), 10 (somewhat), 50 (very), 250 (mandatory) — [Plus Magazine](https://plus.maths.org/content/looking-love); [USC Illumin](https://illumin.usc.edu/the-computation-of-love-finding-your-soul-mate-online/)
- Satisfaction = points earned ÷ points possible over *questions both users answered*. Match % = the nth root of the product of the two satisfaction scores (a geometric mean). Worked example: 98% and 91% give ≈94%. Higher match % ranks a profile higher in search — [Plus Magazine](https://plus.maths.org/content/looking-love)
- The explanation comes from co-founder Christian Rudder's TED-style talk "Inside OkCupid: the math of online dating" — [Amara transcript](https://amara.org/en/videos/8bxZQsHqDDm0/en/66656/)
- Claims about a small-sample penalty (few shared questions capping the shown %) appear in secondary sources, but the excerpt I saw was garbled. Unverified — [AMS Grad Blog](https://blogs.ams.org/mathgradblog/wp-json/wp/v2/posts/29014)

**Tinder**
- Tinder's Spotify integration lets users show top artists and an "Anthem". "Music Mode" auto-plays a profile's Anthem when you view it. A Swedish report says 40% of Gen Z users have connected Spotify (date unclear) — [Android Central](https://androidcentral.com/tinder-music-mode); [M3](https://www.m3.se/article/1850997/tinder-fordjupar-spotify-samarbetet-lanserar-music-mode.html)
- Spotify supplies the API, but Tinder maintains the feature. Users report recurring linking bugs — [Spotify Community](https://community.spotify.com/t5/Other-Podcasts-Partners-etc/Error-Linking-Spotify-to-Tinder/m-p/4640014/highlight/true)

**Timeleft** (dinners for 6 strangers)
- Users take a personality test at signup. Answers plus language, budget and event-type preferences are used to match a group of six. The algorithm also weighs age and gender distribution and aims for an overall "good dynamic" rather than exact age matches — [Timeleft Help](https://help.timeleft.com/hc/en-150/articles/27949262857244-How-Does-Timeleft-Match-Me-With-a-Group); [Timeleft Help](https://help.timeleft.com/hc/en-150/articles/28530361837212-How-Does-Timeleft-Match-Me-With-a-Group)
- Groups are fixed before the event. In mature cities matching is finer; in new cities it relaxes constraints and maximises the number of new encounters — [Timeleft Help](https://help.timeleft.com/hc/en-150/articles/27949262857244-How-Does-Timeleft-Match-Me-With-a-Group)
- Press describes a quiz of roughly 40 questions, e.g. "How important is humor to you?" — [Connexion France](https://www.connexionfrance.com/news/how-to-meet-new-people-over-dinner-in-france-we-try-out-new-app/682998); [Time Out](https://www.timeout.com/usa/news/heres-what-happened-when-i-dined-with-five-strangers-in-austin-031225)

**222**
- A roughly 30-question onboarding survey assigns each user one of 16 personality categories. Weekly $2.22 dinners/events. After an event, participants say whom they'd like to see again *and why*, which feeds back into matching. The company says about 5 experiences are needed before matching adapts. Sources are mostly company statements — [The Star / AFP 2025](https://www.thestar.com.my/tech/tech-news/2025/09/08/ai-powered-meet-up-apps-fight-loneliness); [TechCrunch](https://techcrunch.com/?p=2440702)

**Pie** (Andy Dunn)
- A free local events app. As events grew crowded, connection got harder, so Pie acqui-hired Sparked Connections and added quiz-based matching to select events ("Sparked by Pie"), reportedly using ChatGPT — [TechCrunch 2025](https://techcrunch.com/2025/03/04/andy-dunns-new-app-pie-uses-ai-to-help-you-make-friends); [TechCrunch 2024](https://techcrunch.com/2024/11/19/irl-social-app-pie-is-coming-to-sf-to-make-you-less-lonely)

**IRL** (event/group social app, closed)
- Shut down in June 2023 after a board investigation found about 95% of its claimed ~20M users were likely bots or automated. The SEC charged founder Abraham Shafi in July 2024 with defrauding investors of $170M, alleging that growth claimed to be organic was actually bought through incentivised installs — [TechCrunch](https://techcrunch.com/2024/07/31/founder-behind-social-media-app-irl-charged-with-fraud); [Entrepreneur](https://entrepreneur.com/business-news/irl-messaging-app-shuts-down-95-of-users-were-bots/454821)

### Inferences
- OkCupid's model maps cleanly onto taste. The geometric-mean idea ("both must be satisfied") is a good way to combine directional similarities. Showing a number only after enough shared items (OkCupid computes only over questions both answered) is the important safeguard.
- In dating, music taste works mainly as conversation material (an anthem or shared artists), not as a ranking signal. That is consistent with Last.fm's design and the 2013 research.
- Group apps (Timeleft, 222, Pie) optimise *group composition* under constraints rather than pairwise %. They also learn from post-event "would meet again" feedback, which is a stronger signal than declared taste.

### Gaps
- Hinge: I found no source in this session. Unverified from memory: Hinge lets users connect Spotify to show an anthem or song on the profile and uses prompts; there is no public taste-matching score. Bumble/BFF: Spotify top artists on profile (unverified, not searched). Lex: not researched; Lex is text-first with no known taste scoring.
- OkCupid's current (2025–2026) algorithm may differ from the 2010s description. Not verified.

## 4. Taste/media platforms (Letterboxd, Goodreads, StoryGraph, Criticker, Hunch, TasteDive, taste.io, MovieLens, RYM, Musicboard)

### Takeaway
Few media platforms ship an official pairwise compatibility %. Goodreads has "Compare Books" with a percentage. Criticker has the TCI (Taste Compatibility Index), built from rating distances and used as the engine of its predicted score (PSI). Letterboxd has no native compatibility feature, so third-party tools fill the gap. StoryGraph profiles taste by mood/pace and content tags rather than social similarity. Hunch (acquired by eBay) was the canonical cross-domain "taste graph".

### Cited Findings
- **Goodreads**: "Compare Books" from any user's profile shows shared books and how your tastes match. It can be sorted by average rating or "inverse popularity" to surface obscure shared books. Users report percentages that are usually low, e.g. "never more than 15% compatible" (anecdote) — [Goodreads blog](https://www.goodreads.com/blog/show/247-the-new-compare-books-page); [Book Riot](https://bookriot.com/compare-your-goodreads-reading/)
- **Criticker TCI**: an average of distances between ratings on shared titles. At least 3 shared titles are needed and the threshold rises as you rate more. The TCI is then used to compute the PSI (Probable Score Index) prediction. Sources disagree on scale direction ("lower is better" vs "closer to 1 is closer"), so check Criticker's own docs — [Neowin review](https://www.neowin.net/news/review-criticker-and-their-awesome-film-rating-predictions); [ReadWrite](https://readwrite.com/criticker_movie_recommendations/); [Criticker Info & Guides](https://www.criticker.com/info)
- **Letterboxd**: no native compatibility. Third-party tools fill the gap: friend rankers compute a commonality score from scraped ratings, and "Obscuriboxd" compares how obscure two users' tastes are. This shows clear user demand — [GitHub Letterboxd-friend-ranker](https://github.com/BBottoml/Letterboxd-friend-ranker); [Kottke](https://kottke.org/24/10/0045429-this-simple-tool-finds-yo); [Product Hunt Obscuriboxd](https://www.producthunt.com/products/obscuriboxd/launches)
- **StoryGraph**: discovery filters by mood, pace (slow/medium/fast) and plot vs character-driven. Stats include mood distributions and genre breakdowns. Recommendations come from history, ratings and tags, and the algorithm is not public. Users have publicly requested a "compare taste with any user" feature, so it does not exist yet — [App Store](https://apps.apple.com/app/1570489264); [YourStory](https://yourstory.com/2022/09/app-review-storygraph-mood-based-book-recommendations); [StoryGraph roadmap](https://roadmap.thestorygraph.com/requests-ideas/posts/compare-taste-with-any-user-show-shared-books-and-rating-alignme)
- **Hunch** (founded 2007 by Chris Dixon and Caterina Fake): a "taste graph" whose nodes are people and items and whose edges are taste similarity. Inputs were answers to Hunch's own playful questions plus social network data. Dixon (2011) cited about 500M people, 200M items and 30B edges — [The Next Web](https://thenextweb.com/news/ebay-acquires-taste-graph-startup-hunch-for-a-reported-80m); [Computerworld](https://www.computerworld.com/article/1475102/ebay-buys-e-commerce-recommendations-service-hunch.html)
- eBay acquired Hunch in Nov 2011 for a reported ~$80M (unconfirmed) to improve long-tail recommendations. The public site was promised to stay live, but hunch.com was closed and redirected to eBay in 2014, with the technology absorbed into eBay — [TechCrunch](https://techcrunch.com/2011/11/21/ebayshunch/); [Wikipedia](https://www.wikipedia.org/wiki/Hunch_(website))
- Dixon's thesis: people with similar taste are more relevant than friends for choices like what movie to watch — [Google TechTalk summary](https://rosetta.to/u/googletechtalks/graphs-with-chris-dixon)

### Inferences
- Rating-distance metrics (Criticker) need explicit ratings. Play-count or listen-history sources (Spotify) need implicit-feedback similarity. A cross-domain product will have to normalise both kinds of input.
- The Goodreads anecdote ("never above 15%") shows that raw overlap metrics give discouragingly low numbers in sparse, long-tail catalogues such as books and films. Displayed % probably needs rescaling or calibration (inference).
- Hunch shows that a cross-domain taste graph can be technically valuable (it was acquired) yet fail as a standalone consumer destination.

### Gaps
- Not researched in this session: TasteDive, taste.io, MovieLens (GroupLens research; well documented in academia), Rate Your Music (it has user "compatibility" on profiles — unverified), Musicboard.
- No primary post-mortem from Hunch's founders on why the consumer site ended.

## 5. Event platforms: how they recommend events

### Takeaway
Public detail is thin. Music-event apps (Songkick, Bandsintown, DICE, Spotify Concerts) mostly recommend by **scanning a connected music library or listening history for artists** and matching them to local events, plus location. Partiful's main social signal is **visibility of friends and mutuals attending**. No platform publishes its ranking logic.

### Cited Findings
- Spotify Concerts (2015) launched powered by Songkick, using listening behaviour to recommend shows and updating as taste changes — [TechCrunch](https://techcrunch.com/2015/11/12/spotify-turns-its-recommendation-engine-to-concerts/)
- Spotify's ~13-year Songkick partnership ended and Bandsintown replaced it as the concert-listings partner. Bandsintown also works with Amazon Music and Shazam — [iMusician](https://imusician.pro/en/resources/blog/spotify-integrates-bandsintown-listings-after-its-songkick-partnership-comes-to-an-end)
- Songkick and Bandsintown scan the user's library and listening history, track artists, alert on new shows, and recommend by taste — [tickts.co.uk](https://tickts.co.uk/blog/how-to-find-events-through-spotify-and-music-apps); [TechCrunch (Bandsintown Spotify app)](https://techcrunch.com/?p=748948)
- Spotify's own artist-facing description: a personalised playlist of 30 tracks from artists coming to the fan's town, based on listening history — [Spotify for Artists](https://artists.spotify.com/en/live-events)
- DICE: a "Discover" feed of personalised recommendations. Its privacy policy says it uses location and, with permission, a connected music library for recommendations. Fever and DICE are now combined — [DICE privacy policy](https://support.dice.fm/article/244-uk-privacy-policy); [Fever newsroom](https://newsroom.feverup.com/en-GB/267604-fever-and-dice-one-year-on/)
- Resident Advisor: curated "RA Picks" combined with personalised recommendations by taste and location — [RA about](https://ra.co/about)
- Partiful: guests see which friends are attending; a feed surfaces events friends and mutuals are going to; there is an "interested" RSVP — [App store tracker](https://theapplaunchpad.com/app-store-tracker/1662982304); [Kristen Berman](https://kristenberman.substack.com/p/3-ways-partiful-is-beating-eventbrite)
- PI (pi.fyi) explicitly shows which PI friends are going to an event *and* whether there is strong taste compatibility with other attendees. This is the closest existing analogue to the planned concept — [pi.fyi](https://pi.fyi/p/pi-events-feature)

### Inferences
- The standard pattern is artist-follow plus location matching for music events. "Who is attending" as a ranking signal is mostly social proof (friends and mutuals). Ranking events by attendees' taste similarity is rare, and PI is the only example found.

### Gaps
- No public ranking details for Meetup, Eventbrite, Fever, Shotgun, DICE or RA. Shotgun was not researched. Meetup is believed to recommend by joined groups and topics (not verified).

## 6. Failed or closed taste and social startups

### Takeaway
Closures show recurring patterns. Platform owners don't commit (Ping: Facebook deal collapse, users "didn't want to invest energy"). Taste data is more valuable as infrastructure than as a destination (Hunch absorbed into eBay). Growth was fake or bought (IRL). Narrow single-interest social layers bolted onto a transaction product don't hold up (Ping, Apple Music Connect).

### Cited Findings
- **Apple Ping** (iTunes 10, 2010 to 30 Sep 2012): 1M signups in 48 hours, but low sustained engagement. The Facebook integration was pulled at launch over "onerous terms". Ping felt like a storefront rather than a social network and offered little beyond existing networks. Its successor, Apple Music Connect, also shut down (2018) — [AppleInsider](https://appleinsider.com/articles/18/09/01/apples-failed-ping-showed-how-hard-it-is-to-create-a-music-service); [CNBC](https://www.cnbc.com/2012/06/13/ping-apples-itunes-social-network-will-be-shut-down-this-fall.html); [MacRumors](https://www.macrumors.com/2018/12/13/apple-music-connect-shutting-down); [Macworld](https://www.macworld.com/article/3328859/apple-music-connect-was-doomed-from-the-start.html)
- **Hunch**: acquired in 2011 and its consumer site closed in 2014; see section 4 — [Wikipedia](https://www.wikipedia.org/wiki/Hunch_(website))
- **IRL**: shut down in 2023 after 95% of users turned out to be bots; SEC fraud charges followed; see section 3 — [TechCrunch](https://techcrunch.com/2024/07/31/founder-behind-social-media-app-irl-charged-with-fraud)

### Inferences
- A taste-social product depends on a third-party data source (Spotify, Letterboxd and Goodreads have limited or no APIs), which is platform risk, as Ping showed with Facebook. Taste matching alone is not a retention loop; it needs a recurring real-world action such as events.

### Gaps
- Not researched with sources: Blippy (purchase-sharing network, 2009–2011; known for accidentally exposing credit card numbers, then pivoted or shut down — unverified), Path (closed 2018), Gyminee. Further Spotify API restrictions (Nov 2024 deprecation of audio-features/recommendations endpoints for new apps) are relevant platform risk but were not verified this session.

## 7. How compatibility % is presented and perceived

### Takeaway
Successful presentations pair the number with a concrete *reason*, such as a shared artist or song ("the artist that brings you together", or the shared artist list under Last.fm's bar). They compute only over overlapping items and handle low-overlap pairs gracefully. Opaque or unstable percentages (Blend asymmetry and sudden drops) and very low raw overlaps (Goodreads) frustrate users.

### Cited Findings
- Spotify Blend: % plus "the song/artist that brings you together", shown as a shareable story — [Android Central](https://www.androidcentral.com/spotify-blend-now-official-letting-friends-compare-their-musical-taste); [Spotify Engineering](https://engineering.atspotify.com/2021/12/a-look-behind-blend-the-personalized-playlist-for-youand-you/)
- Last.fm: a colour bar plus a short shared-artist list — [Straight Dope](https://boards.straightdope.com/t/algorithm-for-last-fm/611726)
- OkCupid: % computed only over shared questions and used directly for ranking — [Plus Magazine](https://plus.maths.org/content/looking-love)
- Complaints about Blend % drops and different numbers on each side — [Spotify Community](https://community.spotify.com/t5/Content-Questions/My-taste-match-percentages-in-my-blends-recently-dropped/m-p/6281660)
- Goodreads users find percentages low and treat finding a "ratings twin" as a rare event — [Goodreads blog comments](https://www.goodreads.com/blog/show/247-the-new-compare-books-page); [Confessions of a Book Geek](https://confessionsofabookgeek.com/2017/08/28/have-you-found-your-ratings-twin/)
- Wrapped's Listening Personality presents a type, not a score, with a disclaimer that it is "only a description" — [Spotify Engineering](https://engineering.atspotify.com/2023/1/whats-a-listening-personality)

### Inferences
- Design implications (inference, not sourced fact):
  - make the score symmetric
  - require a minimum overlap before showing it (Criticker requires at least 3 shared titles; OkCupid uses shared questions only)
  - show a per-domain breakdown (music / film / books)
  - always show the top shared items
  - calibrate the display so typical pairs don't look discouraging
  - keep the score stable over time

### Gaps
- No controlled user research on how people perceive taste-% numbers was found in this session.
