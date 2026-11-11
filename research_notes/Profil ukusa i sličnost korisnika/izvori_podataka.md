# Data sources for a user's cultural taste and personality: access conditions (as of Nov 2026)

> Items marked **[BG]** come from my background knowledge (training data to mid-2026). I did NOT re-verify them in this session, so treat them as unverified.

## Music: Spotify, Apple Music, Last.fm, MusicBrainz/ListenBrainz and others

### Takeaway
Spotify is now very hard to use as the main taste source for a new third-party app. Since Nov 2024 the audio features, recommendations and related-artists endpoints are gone for new apps. Since 15 May 2025, extended quota needs an established company and at least 250k MAU. Since Feb/Mar 2026, Development Mode allows 5 users and needs Premium. The Developer Policy also prohibits "analyzing" Spotify content and building user profiles. Good alternatives are Last.fm (open read API, but commercial use needs an agreement), Apple Music (user token, recent and heavy-rotation history), the user's own Spotify data export (GDPR), and MusicBrainz/ListenBrainz for open metadata.

### Cited Findings
**Spotify**
- 27 Nov 2024: Spotify cut new apps off from several recommendation features (audio features, audio analysis, recommendations, related artists, algorithmic/editorial playlists). Apps that already had extended access kept them. — [TechCrunch 2024-11-27](https://techcrunch.com/2024/11/27/spotify-cuts-developer-access-to-several-of-its-recommendation-features)
- 15 Apr 2025 announcement, effective 15 May 2025: Extended Quota Mode is only for "established, scalable and impactful" use cases. Requirements: an established business entity, an active launched service, at least 250k MAU, and availability in key Spotify markets. Apps that already had extended access and comply with the terms are not affected. — [Spotify Dev Blog 2025-04-15](https://developer.spotify.com/blog/2025-04-15-updating-the-criteria-for-web-api-extended-access); [Community thread](https://community.spotify.com/t5/Spotify-for-Developers/Updating-the-Criteria-for-Web-API-Extended-Access/td-p/6920661)
- Third parties say only organizations may apply and review can take up to 6 weeks. This is not confirmed in a primary source. — [vorplabs summary](https://vorplabs.com/agent-tools/spotify-cli)
- 6 Feb 2026: Development Mode now allows **5 users** per app for new apps, and the app owner needs **Premium** (the app stops working if Premium lapses). New apps were affected from **11 Feb 2026** and existing dev-mode apps were migrated **9 Mar 2026**. Extended Quota apps are not affected. — [Spotify Feb 2026 Migration Guide](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide); [Spotify blog 2026-02-06](https://developer.spotify.com/blog/2026-02-06-update-on-developer-access-and-platform-security); [TechCrunch 2026-02-06](https://techcrunch.com/2026/02/06/spotify-changes-developer-mode-api-to-require-premium-accounts-limits-test-users/)
- Endpoints removed or changed in Feb 2026:
  - Removed: batch GET /tracks, /albums, /artists and similar; /browse/new-releases; /browse/categories; /artists/{id}/top-tracks; GET /users/{id}; /users/{id}/playlists; /markets.
  - Playlist /tracks became /items, and items are now visible only for playlists the user owns or collaborates on.
  - Search limit dropped from 50 to 10.
  - The `popularity` field was removed from Track and Artist objects, and the artist `followers` field was removed.
  - The guide does not mention top-items (`/me/top`) or recently-played, so they appear to remain, but this is unconfirmed. — [Migration Guide](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide)
- July 2026: the Client ID cap rose from 1 to 25 per developer account, quota is now counted per developer account, and 429 responses include a reason field. May 2026: an `account_id` field was added to the User object. — [Spotify changelog July 2026](https://developer.spotify.com/documentation/web-api/references/changes/july-2026); [May 2026](https://developer.spotify.com/documentation/web-api/references/changes/may-2026); [Community post](https://community.spotify.com/t5/Spotify-for-Developers/Web-API-quota-updates-for-Development-Mode/td-p/7508852)
- **Developer Terms v10 (effective 15 May 2025):**
  - Using Spotify content to train or feed ML/AI models is prohibited (IV.2.a(i)).
  - No storing or aggregating databases of Spotify Content beyond what the app needs, and no indefinite storage (IV.3.a).
  - Personal data may be processed only as long as needed and within the consent given (V.7).
  - Data must be deleted within 5 days after the user disconnects (App. A §5(c)).
  - Personal data may be used for other purposes only with the user's explicit consent, unless it is only shown back to that same user (V.4).
  - No transfers to ad networks or data brokers "even if a user consents" (IV.5), and no selling of data (V.5). — [Spotify Developer Terms](https://developer.spotify.com/terms)
- **Developer Policy (effective 15 May 2025):**
  - III.13: "Do not analyze the Spotify Content or the Spotify Service for any purpose", including to build user profiles or for ad targeting.
  - III.9: do not build an app that enables transfer of data to another service.
  - IV.2: commercial use is not permitted except limited uses for Non-Streaming SDAs.
  - Matching or dating is not addressed explicitly. — [Spotify Developer Policy](https://developer.spotify.com/policy)

**Apple Music / MusicKit**
- The Apple Music API covers the catalog, ratings, charts, recommendations and the user's recently played content. Personal data needs a developer JWT (maximum 180 days) plus a Music-User-Token. — [Apple Music API docs](https://developer.apple.com/documentation/applemusicapi/get-recently-played-resources.md); [community](https://dev.to/jurooravec/failed-trying-to-get-recently-played-songs-from-apple-music-api-44im)
- Recently played reportedly holds only the last ~50 items, at 10 per request (2021, may be outdated). Developers report intermittent 401 errors. `/me/history/heavy-rotation` exists according to third parties. — [dev.to 2021](https://dev.to/jurooravec/failed-trying-to-get-recently-played-songs-from-apple-music-api-44im); [Apple forums](https://developer.apple.com/forums/thread/777549)

**Last.fm**
- Methods like `user.getTopArtists` (periods overall/7day/1month/3month/6month/12month) work on **public usernames** with an API key, so no OAuth is needed to read history. — [Last.fm API docs](https://www.last.fm/api/show/user.getTopArtists)
- Commercial use requires a commercial use agreement. Using the data commercially without one is a material breach, and Last.fm reserves a share of revenue. Contact: partners@last.fm. — [Last.fm API ToS](https://www.last.fm/api/tos)
- The rate limit is unclear: third parties quote "5 req/s", but this is unverified. — [lastfm-docs](https://lastfm-docs.github.io/api-docs)

**MusicBrainz / ListenBrainz / AcousticBrainz**
- AcousticBrainz stopped taking submissions in 2022 and its dataset was released as a final dump. MetaBrainz planned to move its functionality into ListenBrainz. — [MetaBrainz blog](https://community.metabrainz.org/t/acousticbrainz-making-a-hard-decision-to-end-the-project/572828)
- ListenBrainz had server issues and high load in June–July 2026. — [MetaBrainz blog feed](https://rss-parrot.net/web/feeds/blog.metabrainz.org)

**Spotify data export (GDPR)**
- "Extended streaming history" covers songs, videos and podcasts for the whole lifetime of the account. It arrives as a ZIP of JSON files. — [Spotify support](https://support.spotify.com/us/article/understanding-your-data/)
- Third parties say Spotify quotes up to 30 days for delivery, and users report anywhere from 1 day to 2 weeks. The basic account data takes about 5 days. — [freeyourmusic](https://freeyourmusic.com/blog/spotify-extended-streaming-history); [ericchiang](https://ericchiang.github.io/post/spotify)

### Inferences
- A Croatian events startup will not reach 250k MAU at launch. The Spotify Web API therefore gives it at most 5 users, which only works for a prototype. The practical routes are the user uploading their own Spotify export ZIP (GDPR Art. 15/20, processed under our own legal basis, outside the Spotify Developer Terms) or Last.fm/Apple Music.
- Even with extended quota, the Policy's "do not analyze... build user profiles" clause directly conflicts with taste-matching. Get legal review before building any Spotify-API-based profiling. The user-upload route avoids the Spotify developer agreement, but GDPR still applies in full.
- Last.fm is the richest open listening history (scrobbles with timestamps plus folksonomy tags). However, it is used by a small share of users, and a commercial agreement is needed.
- **[BG]** Other music services, none re-verified in this session:
  - **Deezer API**: OAuth with `listening_history`. It offers user favorites, history and flow, and is relevant in the EU.
  - **YouTube Data API v3**: `subscriptions.list` (mine=true) and liked videos (the LL playlist) work, but watch history has not been available since 2016.
  - **YouTube Music**: no official API.
  - **SoundCloud**: API registration has been reopened only intermittently.
  - **Bandcamp**: no public fan API. Public fan collection pages exist, but scraping them carries ToS risk.
  - **Tidal**: a developer platform (2024+) exists, but user-collection scopes are limited.
  - **Discogs**: API with public user collections and wantlists, and data dumps under CC0.
  - **MusicBrainz** core data: CC0.
  - **ListenBrainz**: open API, can import from Last.fm and Spotify, data released openly.

### Gaps
- I could not confirm whether Spotify's `/me/top/{type}`, `/me/player/recently-played` and the saved-tracks read endpoints are still available in dev mode after March 2026. The migration guide links to a full list that I did not read.
- Apple Music heavy-rotation and history limits were not verified against current Apple docs.
- Current registration status and terms for Deezer, YouTube, SoundCloud, Tidal, Bandcamp and Discogs were not verified this session.

## Film/TV: Letterboxd, IMDb, Trakt, TMDB and others

### Takeaway
There is no open, user-authorized API for Letterboxd (access by request only) or IMDb (no API). The realistic routes are user exports (Letterboxd CSV, IMDb ratings/lists CSV) and public RSS. Trakt has an open OAuth API, but free-tier limits were tightened in 2025. TMDB and IMDb datasets are **non-commercial by default**, so commercial use needs a separate agreement.

### Cited Findings
- **Letterboxd**: API access is by request only (email describing the use). They may not reply or grant access. It uses OAuth2 (Authorization Code, Client Credentials, Refresh), with the Password flow reserved for first-party use. For non-Letterboxd film metadata they point to TMDB. — [Letterboxd API Access](https://letterboxd.com/api-beta/); [API docs](https://api-docs.letterboxd.com)
- **IMDb datasets**: offered for "personal and non-commercial use" only. They must not be republished or repurposed to build a movie database, and permission can be withdrawn. Commercial use goes through IMDb content licensing. — [IMDb Non-Commercial Datasets](https://developer.imdb.com/non-commercial-datasets); [IMDb Help](https://help.imdb.com/article/imdb/general-information/can-i-use-imdb-data-in-my-software/G5JTRESSHJBBHTGX)
- **TMDB**: the licence "does not permit any commercial use" without a separate written agreement, and attribution is required. Some third-party blogs say commercial use is fine with attribution, which contradicts the official terms. — [TMDB API Terms](https://themoviedb.org/api-terms-of-use); [TMDB FAQ](https://developer.themoviedb.org/docs/faq)
- **Trakt**:
  - Feb 2025: stricter free limits (lists and watchlist capped at 100) and a VIP price increase. From 20 May 2025, all VIP renewals moved to a single higher price. — [AlternativeTo 2025-02](https://alternativeto.net/news/2025/2/trakt-tv-has-set-stricter-limits-for-free-users-and-raised-vip-subscription-prices-by-100-); [AlternativeTo 2025-05](https://alternativeto.net/news/2025/5/trakt-announces-all-vip-renewals-will-switch-to-a-new-standard-rate-doubling-prices)
  - 2026: free limits raised to 250 watchlist items and 250 items per list. The API returns HTTP 420 when an account is over its limit. A Fair Use Policy for developers is in draft. — [Trakt forums: Limits 2026](https://forums.trakt.tv/t/updating-trakt-limits-for-2026/101592); [trakt-api #350](https://github.com/trakt/trakt-api/discussions/350); [Fair use draft](https://forums.trakt.tv/t/draft-trakt-fair-use-policy-feedback-welcome/100842)
  - Unverified report: free accounts may be limited to 1 connected app at a time, so a user already linked to Plex or Kodi would have to drop that link for ours. — [troypoint (secondary)](https://troypoint.com/?p=457069)

### Inferences
- **[BG]**:
  - Letterboxd has an RSS feed at `letterboxd.com/<user>/rss/` with recent diary entries and ratings (TMDB IDs in the `tmdb:` namespace), plus a user ZIP export (watched, ratings, diary, reviews, watchlist, likes CSV). These are probably the most practical channels.
  - IMDb lets users export their ratings and lists to CSV.
  - Simkl has a free OAuth API with an anime/TV/film watchlist.
  - JustWatch has no public API, only partner/B2B.
  - Netflix: "Download your information" (GDPR) includes ViewingActivity.csv, and Account → Viewing activity exports a CSV of titles. There is no API.
- Wikidata (CC0) is the commercially safe enrichment layer for IDs and genres. It maps IMDb, TMDB and MusicBrainz identifiers, which avoids the TMDB/IMDb licence problems.

### Gaps
- No primary-source confirmation for the current Letterboxd RSS and export format, or for Simkl and JustWatch terms.
- TMDB commercial licensing prices were not found.

## Books: Goodreads, StoryGraph, Open Library, Google Books, Hardcover

### Takeaway
Goodreads (API closed to new keys since Dec 2020 **[BG]**) and StoryGraph have no API but both offer CSV export. Hardcover has a public (GraphQL) API and CSV export/import. Open Library and Google Books are for metadata enrichment.

### Cited Findings
- The Goodreads CSV export comes from Import/Export → "Export Library". StoryGraph imports Goodreads CSVs and offers its own CSV export (Settings → Export). — [digitaltrends](https://qa2.www.digitaltrends.com/mobile/how-to-transfer-your-books-from-goodreads-to-the-storygraph); [Literal](https://literal.club/storygraph-alternatives)
- Hardcover exports CSV (hardcover.app/account/exports) and added a custom CSV import in March 2025. It exposes a public API for builders. — [Hardcover report March 2025](https://static.hardcover.app/blog/hardcover-report-for-march-2025); [Literal vs Hardcover](https://literal.club/vs/hardcover)

### Inferences
- **[BG]**:
  - Goodreads RSS shelves (`goodreads.com/review/list_rss/<id>?shelf=read`) still work for public profiles, with ratings.
  - Open Library API and data dumps are open (CC0 for the data).
  - Google Books API needs an API key and is mainly for metadata. Its user "bookshelves" are rarely used.
- Book ratings and shelves are a strong Openness signal. The Goodreads CSV includes My Rating, shelves, read dates and reviews.

### Gaps
- Exact Hardcover API terms (commercial use, rate limits) were not verified.

## Other domains: games, sport, social, anime, board games and more

### Takeaway
Meta (Instagram/Facebook) no longer lets consumer apps read personal accounts' likes or interests. Strava forbids showing a user's data to other users and forbids AI use. The rest (Steam, AniList/MAL, BGG, Reddit, Twitch, Discord) generally have OAuth or public APIs, but with ToS limits.

### Cited Findings
- **Instagram Basic Display API** shut down on 4 Dec 2024. There is no longer an Instagram API for personal consumer accounts; the Instagram API with Instagram Login covers business and creator accounts only. — [Meta Dev Blog 2024-09-04](https://developers.facebook.com/blog/post/2024/09/04/update-on-instagram-basic-display-api)
- **Strava** (API agreement update, Nov 2024):
  - Apps may show a user's data only to that same user, never to other users.
  - Using API data in AI models is prohibited.
  - In June 2026 AI use was tightened further (written guidance from Strava is required) and routing through intermediary platforms is no longer supported. — [DC Rainmaker](https://dcrainmaker.com/2024/11/stravas-changes-to-kill-off-apps.html); [TechRadar](https://techradar.com/health-fitness/strava-could-soon-stop-working-with-some-of-your-third-party-apps-heres-what-you-need-to-know); [TechRepublic 2026](https://www.techrepublic.com/article/news-strava-api-scraping-crackdown/)
- **Google Data Portability API (DMA Art. 6(9))**: scopes include `dataportability.myactivity.youtube` (classed as Restricted), plus YouTube channel, subscriptions and playlists scopes.
  - Restricted scopes require verification and a security assessment.
  - Users under 18 are not supported, and availability must be checked per user location.
  - The EC DMA portal describes authorisations of up to 180 days, free of charge. — [Google DPAPI scopes](https://developers.google.com/data-portability/user-guide/scopes); [EC DMA portal](https://digital-markets-act.ec.europa.eu/developer-portal/end-user-data-portability_en)

### Inferences
- **For the EU, the Google Data Portability API is the main exception**: it is the only legal way to get YouTube watch history, search and My Activity programmatically. However, the restricted-scope security assessment (CASA) is expensive and slow.
- Under DMA Art. 6(9), Meta must also offer EU portability (Download Your Information / data transfer to authorized third parties), but no public API comparable to Google's was confirmed.
- **[BG]**, not re-verified:
  - **Steam Web API**: `GetOwnedGames` returns playtime when the profile is public. A key is required, and the Steam Web API ToU limits requests (about 100k/day).
  - **AniList**: open GraphQL API with OAuth. Lists are public by default.
  - **MyAnimeList API v2**: OAuth2 with PKCE.
  - **BoardGameGeek XML API2**: public collections with ratings. Since 2025 it requires registration and an application token.
  - **Reddit**: API terms from 2023. Paid above free limits, and commercial use needs approval.
  - **Twitch Helix**: `user:read:follows` returns followed channels.
  - **Discord OAuth**: `identify`, `guilds` (server list). The Discord developer policy forbids profiling.
  - **Pinterest API v5**: boards and pins with OAuth, but access requires approval.
  - **Untappd**: API registration is closed to new apps.
  - **Duolingo**: no official API.
  - **Pocket**: shut down in July 2025.
  - **TasteAtlas**: no public API.
  - **Museums**: open collection APIs (Rijksmuseum, Europeana, The Met). These are for item metadata, not user taste.
- Facebook page likes (`user_likes`) are the classic strongest signal for personality (Kosinski 2013), but they are probably practically unavailable to new apps after App Review. This is unconfirmed.

### Gaps
- `user_likes` status on the Facebook Graph API in 2025–2026 was not verified.
- The 2025 BGG token requirement, Untappd and Pocket status were not verified in this session.

## Questionnaires (Big Five, STOMP-R, MUSIC) and their licences

### Takeaway
TIPI is free to use. The BFI-2 (and probably the BFI-2-S/XS) is **non-commercial only**, so commercial use needs Berkeley lab permission. IPIP instruments (Mini-IPIP, IPIP-NEO) are public domain **[BG]** and the safest commercial choice.

### Cited Findings
- TIPI is free for any use without permission. It is intended for group-level research, not individual high-stakes decisions. — [Testable TIPI](https://www.testable.org/scale/tipi-ten-item-personality-inventory-big-five)
- The BFI-2 is freely available for non-commercial research only. "At this time, the BFI-2 is for non-commercial uses only", and commercial requests go to ucbpersonalitylab@gmail.com. Whether the BFI-2-S follows the same terms is unconfirmed. — [Berkeley John Lab BFI-2](https://www.ocf.berkeley.edu/~johnlab/bfi.html)
- The BFI-S (15 items, SOEP, Gerlitz & Schupp) is a different instrument and is free for research and education. — [Testable BFI-S](https://www.testable.org/scale/bfi-s-big-five-inventory-short-form-15-items)

### Inferences
- **[BG]**:
  - IPIP is public domain (ipip.ori.org), including the Mini-IPIP (20 items) and IPIP-NEO-120.
  - STOMP-R (Rentfrow & Gosling 2003) and the MUSIC model (Rentfrow et al. 2011/2012), including the audio-clip version, are freely available on Gosling's and Rentfrow's lab sites for research. Commercial status is not explicit, so ask the authors.
  - A Croatian translation and validation exist for some instruments (e.g. IPIP-50 HR) but should be checked.

### Gaps
- No primary licence text was found for the BFI-2-S, BFI-2-XS, STOMP-R or MUSIC short forms.

## Which sources carry the most predictive signal for personality and taste?

### Takeaway
Digital-footprint predictions of the Big Five reach r ≈ 0.29–0.40 (meta-analysis). Music listening behaviour from streaming predicts the Big Five with "moderate to high" accuracy. Likes are historically the strongest single social signal, and combining several types of footprint improves accuracy.

### Cited Findings
- Meta-analysis (Azucar, Marengo & Settanni 2018, PAID 124:150–159): r ranges from 0.29 (Agreeableness) to 0.40 (Extraversion). Accuracy improves when models combine several types of digital footprint with demographics. — [Azucar et al. 2018 (Scribd copy)](https://www.scribd.com/document/607208542/Azucar-et-al-2018); [UniTo repository](https://iris.unito.it/handle/2318/1774688)
- Anderson et al. 2021, SPPS 12(4):561–572: 5,808 Spotify users, 17.6M songs, 662k hours over 3 months, 211 metrics. The Big Five were predicted with "moderate to high accuracy". The lead author is from Spotify, a possible conflict of interest. — [Bar-Ilan CRIS](https://cris.biu.ac.il/en/publications/just-the-way-you-are-linking-music-listening-on-spotify-and-perso/); [PDF](https://Www.Gwern.net/doc/psychology/personality/conscientiousness/2020-anderson.pdf)

### Inferences
- **[BG]**:
  - Kosinski, Stillwell & Graepel 2013 (PNAS) found that Facebook likes predicted Openness at r ≈ 0.43.
  - Youyou et al. 2015 (PNAS) found that about 300 likes beat a spouse's judgement of personality.
  - Openness is the most predictable trait from cultural consumption.
  - Behavioural listening data (timing, variety, routines) adds signal beyond genre preference alone.
- For matching people at events, taste overlap itself (shared artists and genres) is probably more useful than a derived Big Five profile.

### Gaps
- No direct comparison (music vs film vs books) was found within a single study.

## Legal: GDPR profiling, special categories and platform terms

### Takeaway
Taste data can **indirectly reveal** special categories (religion, politics, sexual orientation), and the CJEU treats such inferences as Art. 9 data. Profiling for matching therefore needs explicit consent and minimisation or filtering of sensitive inferences. The platforms (Spotify, Strava, Last.fm, TMDB) add their own restrictions on top of GDPR.

### Cited Findings
- CJEU C-184/20 (1 Aug 2022): data that can **indirectly** reveal sexual orientation falls under Art. 9. The controller's intent and the data not being inherently sensitive are irrelevant. — [Inside Privacy (Covington)](https://www.insideprivacy.com/eu-data-protection/special-category-data-by-inference-cjeu-significantly-expands-the-scope-of-article-9-gdpr/); [Helbing summary](https://www.thomashelbing.com/en/wissen/dsgvo-hub/rechtsprechung/1.4.15-eugh-vyriausioji)
- C-252/21 Meta v Bundeskartellamt (4 Jul 2023) is a landmark case on Art. 6(1)(b)/(f) and its interaction with Art. 9. The details were not confirmed in this session. — [ceelegalmatters](https://ceelegalmatters.com/romania/20843-cjeu-strikes-again-this-time-targeting-special-data)
- Spotify forbids analysing content to build user profiles and forbids transferring data to ad and data-broker tools even with user consent. Strava forbids showing a user's data to others and forbids AI use. — [Spotify Policy](https://developer.spotify.com/policy); [Spotify Terms](https://developer.spotify.com/terms); [DC Rainmaker](https://dcrainmaker.com/2024/11/stravas-changes-to-kill-off-apps.html)
- DMA Art. 6(9) gives EU users continuous, free portability from gatekeepers to authorised third parties. — [EC DMA portal](https://digital-markets-act.ec.europa.eu/developer-portal/end-user-data-portability_en)

### Inferences
- **[BG]**:
  - Art. 20 GDPR (portability) only obliges the source controller to hand data to the user. When the user uploads an export to us, we need our own legal basis (consent, Art. 6(1)(a)) and Art. 9(2)(a) explicit consent if we infer sensitive traits.
  - The EDPB/WP29 profiling guidelines (WP251rev.01) and portability guidelines (WP242rev.01) apply.
  - A DPIA (Art. 35) is likely required: systematic profiling, matching, and possible sensitive inferences.
  - Art. 22 is likely not triggered for event recommendations, but transparency obligations (Art. 13(2)(f)) apply.
  - Croatian supervisory authority: AZOP.
- Practical mitigations:
  - Exclude or not store genres and items that reveal religion or politics (e.g. religious music, political podcasts, LGBTQ+ venues and events) unless explicit consent is given.
  - Do not show other users raw history, only aggregated overlap.
  - Delete source data after extracting features. This is required for Spotify (5 days after disconnect) and consistent with minimisation.

### Gaps
- No current EDPB guidance specifically on taste-based matching was found.
- AZOP practice on similar apps was not researched.
