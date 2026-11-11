# Reading ONE shared public list per platform (status as of Nov 2026)

Scope: can a new third-party consumer app (EU/Croatia event social network) read a single public list/playlist a user shares as a link, to build a taste profile and compute user similarity? Research done 2026-11-11 with ~16 searches/fetches. Items marked **[BK-unverified]** are from background knowledge and were NOT re-verified in this session.

## Summary table (verification level noted per cell)

| Platform | (a) Official way to read one shared list | (b) Fields | (c) Quotas / gating for new apps | (d) ToS relevant to taste profiles | (e) Fallback |
|---|---|---|---|---|---|
| Spotify | `GET /playlists/{id}` + `/playlists/{id}/items`; Dev Mode apps get **metadata only, no items** for playlists the user neither owns nor collaborates on (since 11 Feb 2026 new apps / 9 Mar 2026 existing) — verified | Track IDs, names, artists; `external_ids` (ISRC) removed in Feb then **reverted** in March 2026 — verified | Dev Mode: 5 users, owner needs Premium; Extended quota: registered organisation, ≥250k MAU, key markets — verified | Spotify Developer Policy bans using Spotify data to train ML / build profiles beyond app use **[BK-unverified]** | Have the sharing user OAuth-connect (owner reads own playlist — still capped at 5 users in Dev Mode); oEmbed gives title/thumbnail only **[BK-unverified]**; manual paste + MusicBrainz match |
| Apple Music | Catalog playlist endpoint with developer token only; `pl.u-` shared user playlists reportedly readable via catalog call using `globalId` — anecdotal | Songs with ISRC in catalog song attributes **[BK-unverified]** | Apple Developer Program membership needed **[BK-unverified]** | Apple Music API terms restrict data use **[BK-unverified]** | Manual paste / in-app build |
| Deezer | Public REST `/playlist/{id}` historically key-less; API **disabled mid-2025**, status unclear — partly verified | Title, creator, fans, track count, tracks (ISRC on track objects **[BK-unverified]**) | Unknown; no official reopening notice found | Unknown | Same |
| YouTube / YT Music | `playlistItems.list` (1 unit per page of 50); public playlist with API key **[BK-unverified that key suffices]** | Video IDs, titles, channel; no ISRC | 10,000 units/day default — verified | YouTube API Services ToS/Developer Policies (data retention, no profile building beyond service) **[BK-unverified]** | Title-string matching to MusicBrainz |
| Tidal | New JSON:API platform `openapi.tidal.com/v2`; `playlists.read` scope — partly verified | Catalog incl. ISRC **[BK-unverified]** | Unknown | Unknown | — |
| Letterboxd | API by request only; per-member RSS covers diary, reviews and **lists** — verified | Film titles, Letterboxd links; RSS items carry TMDB ID **[BK-unverified]** | Approval-gated | Terms forbid scrapers/robots "except as explicitly authorized" (terms updated Dec 2025) — verified | RSS of list owner; CSV export (user upload) |
| Trakt | Public list items endpoint `/users/{id}/lists/{list_id}/items` **[BK-unverified path]** | IMDb, TMDB, TVDB, Trakt IDs **[BK-unverified]** | Draft fair-use policy; 2026 per-user list limits — verified | Commercial terms not found | — |
| TMDB | User lists API v3/v4 **[BK-unverified]**; mainly used as catalogue | IDs, IMDb ID via external_ids | — | **Commercial use requires a written agreement**; attribution notice required — verified | Canonical film/TV catalogue for in-app list building (needs commercial licence if app earns revenue) |
| Goodreads | API: no new keys since Dec 2020 — verified; shelf RSS still reported working (~100 items) — community only | Title, author, ISBN in RSS **[BK-unverified]** | n/a | Scraping/RSS ToS not verified | User CSV export upload — verified as common fallback |
| Hardcover | GraphQL `api.hardcover.app/v1/graphql`, user Bearer token — community docs | Book IDs, ISBNs **[BK-unverified]** | ~60 req/min (unofficial) | Reportedly: user-owned data (incl. lists) not for commercial products unless acting for the user with explicit consent — secondhand | — |
| BoardGameGeek | XML API2 (collection, geeklist) now needs **registered app + Bearer token** (since ~fall 2025) — community-verified | BGG IDs, names | Approval can take a week+ | **Commercial use requires commercial licence** — verified (BGG wiki) | — |
| Steam | `IWishlistService/GetWishlist` with standard Web API key on public profile — community only | appIDs + priority | Standard Web API key | Steam Web API ToU **[BK-unverified]** | — |
| IGDB | Catalogue only (Twitch client credentials) | — | — | Commercial use: older "free under Twitch DSA"; Sept 2026 third-party page says commercial partnership needed — conflicting | — |

## (a) Music platforms: can a new app read a single shared public playlist?

### Takeaway
Spotify — the most important platform — no longer lets a new Development Mode app read the **contents** of a playlist the authenticated user does not own or collaborate on (since 11 Feb 2026 for new apps, 9 Mar 2026 for existing). Combined with the 5-user cap and the 250k-MAU/organisation bar for Extended Quota, a new small EU startup effectively cannot read arbitrary shared Spotify playlists via the official API. The only official path left in Dev Mode is the playlist owner authenticating (owner's own playlist), which is capped at 5 users.

### Cited Findings
- Spotify changelog (Feb 2026): playlist `tracks` field renamed to `items`; "Will only return an items object for the user's playlist, other playlists will only provide metadata." — [Spotify Web API Changelog Feb 2026](https://developer.spotify.com/documentation/web-api/references/changes/february-2026)
- Migration guide: "Playlist contents (`items`) are only returned for playlists the user owns or collaborates on." "For other playlists, only metadata is returned and the `items` field will be absent from the response." — [Spotify Feb 2026 migration guide](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide)
- Dates: 11 Feb 2026 "New Development Mode apps are created with new restrictions"; 9 Mar 2026 "Existing Development Mode apps are migrated to new restrictions." — [migration guide](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide)
- "All Development Mode apps require the app owner to have an active Spotify Premium subscription"; new apps limited to 5 users; "Apps in extended quota mode are not affected by any of the changes." Client IDs per developer raised to 25 in July 2026. — [migration guide](https://developer.spotify.com/documentation/web-api/tutorials/february-2026-migration-guide)
- `GET /playlists/{id}/tracks` removed and replaced by `/playlists/{id}/items`; "Get User's Playlists" (for another user), "Get User's Profile", "Get Several Tracks/Albums" removed; `popularity` and `followers` removed; Search `limit` max reduced 50→10. `external_ids` (ISRC/UPC) removed in Feb but marked "reverted in the March 2026 changelog". — [Spotify changelog](https://developer.spotify.com/documentation/web-api/references/changes/february-2026)
- Extended Quota criteria: legally registered business/organisation, launched service, ≥250k MAU, available in key Spotify markets, commercial viability; applications from organisations only since 15 May 2025; no guarantee of approval. — [Spotify Quota modes](https://developer.spotify.com/documentation/web-api/concepts/quota-modes); [Spotify community thread](https://community.spotify.com/t5/Spotify-for-Developers/Updating-the-Criteria-for-Web-API-Extended-Access/td-p/6920661/page/8)
- Community reports of breakage (playlists filtered out, 403s) after the change. — [spotify-mcp-server issue #35](https://github.com/marcelmarais/spotify-mcp-server/issues/35); [rspotify issue #550](https://github.com/ramsayleung/rspotify/issues/550)
- A July 2026 "Web API quota updates for Development Mode" forum thread exists; content not verified. — [Spotify community](https://community.spotify.com/t5/Spotify-for-Developers/Web-API-quota-updates-for-Development-Mode/m-p/7555380/highlight/true)
- Apple Music: developer forum answer says another user's playlist can be fetched via "Get a Catalog Playlist" using `attributes.playParams.globalId` instead of the library `id`, with developer token only, no user sign-in (anecdotal, older thread). — [Apple Developer Forums thread 123894](https://developer.apple.com/forums/thread/123894)
- Apple Music: catalog endpoints need only developer token; library endpoints need `Music-User-Token`. — [skills.sh community guide](https://skills.sh/epheterson) (secondary)
- Apple Music: developers reported HTTP 500/401 on catalog endpoints with valid developer token (2025). — [Apple forums thread 804023](https://developer.apple.com/forums/thread/804023)
- Deezer: community thread June 2025 says the API "was disabled for the time being"; community manager: working on "new ways of ensuring that our services are used properly, which will enable us to reconsider the conditions of access". No official notice found. — [Deezer community thread 80857](https://en.deezercommunity.com/features-feedback-44/api-auth-impossible-80857)
- YouTube Data API: `playlistItems.list` costs 1 unit; default quota 10,000 units/day; each page of up to 50 results charged. — [Google: Determine quota cost](https://developers.google.com/youtube/v3/determine_quota_cost)
- Tidal: official developer platform is a JSON:API at `openapi.tidal.com/v2` with playlist endpoints and `playlists.read` scope; private/unofficial API "not designed to be publicly accessible". — [apis.io TIDAL Playlists API](https://apis.io/apis/tidal/tidal-playlists-api/); [minim docs](https://minim.readthedocs.io/en/dev/api-ref/minim.api.tidal.PlaylistsAPI.html) (secondary)

### Inferences
- For Spotify, the realistic options for a new app are: (1) the sharing user connects Spotify and the app reads that user's **own** playlist (works in Dev Mode but max 5 users total — not viable for launch); (2) apply for Extended Quota (requires 250k MAU — not viable for a new startup); (3) non-API fallback: user pastes/exports track list (e.g. copy-paste from desktop client gives track links **[BK-unverified]**) or builds list in-app from MusicBrainz. Spotify oEmbed (`open.spotify.com/oembed`) returns only title/thumbnail/embed HTML, not track list **[BK-unverified]**.
- Spotify Developer Policy historically prohibits using Spotify content to train ML models and restricts creating user profiles/analytics from Spotify data for purposes beyond the app experience **[BK-unverified — check current policy text]**.
- Apple Music: a `pl.u-` link is a user-shared library playlist; whether developer-token-only reads still work in 2026 is unverified — must be tested. Apple catalog song objects include `isrc` attribute **[BK-unverified]**.
- Deezer historically allowed key-less GET `api.deezer.com/playlist/{id}` for public playlists returning tracks with ISRC **[BK-unverified]**; whether this still works in late 2026 is unknown.
- YouTube Data API v3 `playlistItems.list` works with an API key (no OAuth) for public/unlisted playlists **[BK-unverified]**; returns video IDs/titles only — music identity requires fuzzy matching. YouTube Music has no separate official API **[BK-unverified]**.
- SoundCloud: new API app registration has been intermittently closed for years; reopened with a form at some point **[BK-unverified]**. Bandcamp: no public consumer API (only label/merch APIs) **[BK-unverified]**. Amazon Music: Web API in closed beta / partner-only **[BK-unverified]**. Qobuz: no open public API; app IDs by agreement **[BK-unverified]**. Last.fm: no playlists in API; `user.getLovedTracks` / `user.getTopTracks` with API key only (non-commercial ToS; commercial use needs permission) **[BK-unverified]** — note this reads history, which is outside the "one list" scope but a user could treat "loved tracks" as their list.

### Gaps
- Could not verify Spotify oEmbed fields or current Developer Policy wording on profiling/ML.
- Could not verify whether client-credentials token can GET `/playlists/{id}/items` for any public playlist in Dev Mode (the guide speaks of "the user"; client credentials has no user, so likely metadata-only). Needs a live test.
- Apple Music `pl.u-` handling, Tidal public playlist read without user auth, Deezer API status, SoundCloud/Amazon/Qobuz/Bandcamp — not verified with 2026 sources.

## (b) Film/TV platforms

### Takeaway
Letterboxd is the most culturally relevant but the API is request-only and its terms ban scraping; the sanctioned automated route is the member RSS feed (which includes lists) or a user-uploaded export. Trakt and TMDB have real list APIs with rich external IDs, but TMDB requires a written agreement for any commercial use, and Trakt's commercial terms were not found.

### Cited Findings
- Letterboxd: "Access to the Letterboxd API is available by request only"; every member profile has an RSS feed of new diary entries, reviews **and lists**; Letterboxd recommends applying to TMDB for movie metadata. — [Letterboxd API Access](https://letterboxd.com/api-beta/)
- Letterboxd Terms: must not use "any robot, spider, scraper, deep-link, or other automated data gathering or extraction tool" except as explicitly authorized; terms updated Dec 2025. — [Letterboxd Terms](https://letterboxd.com/terms)
- Letterboxd RSS feed size reported 20–50 recent items (third-party claims, conflicting). — [logiover guide](https://logiover.pages.dev/guides/letterboxd-no-api-export-film-review-data/)
- TMDB: licence "does not permit any commercial use of TMDB, the TMDB APIs, or TMDB Content"; commercial users must enter a written agreement. FAQ: commercial if "primary purpose is to create revenue for the benefit of the owner"; attribution notice "This product uses the TMDB API but is not endorsed or certified by TMDB." — [TMDB API Terms](https://themoviedb.org:443/api-terms-of-use); [TMDB FAQ](https://developer.themoviedb.org/docs/faq)
- TMDB commercial licence reported at $149/month for companies under $1M revenue — third-party, unconfirmed. — [api.market blog](https://api.market/blog/sleeyax/entertainment/best-movie-apis)
- Trakt: 2026 limits — personal lists Free 5 / VIP 100; items per list 250 / 5K; watchlist 250 / 5K. Draft fair-use policy with progressive enforcement. — [Trakt forum: Updating Trakt Limits for 2026](https://forums.trakt.tv/t/updating-trakt-limits-for-2026/101592); [Trakt draft fair use policy](https://forums.trakt.tv/t/draft-trakt-fair-use-policy-feedback-welcome/100842)
- Feb 2025 Trakt free tier cut (2 lists x 100 items at that time). — [AlternativeTo news](https://alternativeto.net/news/2025/2/trakt-tv-has-set-stricter-limits-for-free-users-and-raised-vip-subscription-prices-by-100-)

### Inferences
- Letterboxd list RSS items carry `tmdb:movieId` tags **[BK-unverified]**, which would give clean TMDB IDs; but automated fetching of RSS for a different purpose (profiling) may still be argued as "automated extraction" — legal review needed.
- Trakt public list items are readable with only the app's `trakt-api-key` header (client ID, no user OAuth) and return Trakt/IMDb/TMDB/TVDB IDs **[BK-unverified]**.
- IMDb: user lists are exportable as CSV by owner; IMDb Conditions of Use prohibit data mining/robots; official IMDb data is via AWS Data Exchange (paid) or non-commercial datasets **[BK-unverified]**.
- MUBI, JustWatch, Serializd: no public API **[BK-unverified]**. Simkl: has a public API with client ID **[BK-unverified]**.

### Gaps
- Letterboxd list-RSS URL format and whether a single list (vs. all of a member's lists) has its own feed — not verified.
- Trakt API commercial-use terms not found.

## (c) Books

### Takeaway
No major book platform offers a clean, commercially licensed "read one shared list" API. Goodreads closed new API keys in Dec 2020; shelf RSS reportedly still works but is unofficial. Hardcover has a usable GraphQL API but reportedly forbids commercial use of user list data unless acting on behalf of a consenting user. Open Library lists are the cleanest open option.

### Cited Findings
- Goodreads stopped issuing new API keys Dec 2020 and planned to retire API. — [Infodocket 2020](https://www.infodocket.com/2020/12/13/report-goodreads-plans-to-retire-api-access-disables-existing-api-keys/); [Jane Friedman](https://janefriedman.com/goodreads-closing-off-access-to-its-data-making-it-less-useful/)
- Goodreads shelf RSS reportedly still works, ~100 recent items per shelf; CSV export is the more reliable fallback (third-party). — [gatsby-source-goodreads](https://github.com/eligundry/gatsby-source-goodreads); [Goodreads MCP](https://glama.ai/mcp/servers/w2kfmsb5g1)
- Hardcover: GraphQL at `https://api.hardcover.app/v1/graphql` with user Bearer token from settings; API in beta; user-owned data incl. lists "may not be used in commercial products unless you act for a user who has explicitly granted access"; public image display requires DMCA policy (secondhand). — [glama.ai Hardcover MCP](https://glama.ai/mcp/servers/jn6wxtio67); [emgoto Hardcover API guide](https://master--emgoto.netlify.app/hardcover-book-api)
- Hardcover rate limit ~60 req/min, max query depth 3 (unofficial). — [ClawKit](https://getclawkit.com/skills/official-asaphko-hardcover)

### Inferences
- The StoryGraph: no public API; offers user CSV export **[BK-unverified]**.
- Open Library: lists API (`/people/{user}/lists/{id}/seeds.json`) is public, no key; data is open (bulk dumps CC0) **[BK-unverified]**.
- BookWyrm: federated (ActivityPub); shelves/lists exposed as ActivityStreams JSON per instance **[BK-unverified]**.

### Gaps
- Official Hardcover API terms page and Goodreads ToS on RSS consumption not verified.

## (d) Games, board games, others

### Takeaway
BoardGameGeek now requires registered apps with Bearer tokens and a commercial licence for commercial use. Steam public wishlists appear readable with a standard Web API key. IGDB commercial terms are contested.

### Cited Findings
- BGG XML API requires registered application at boardgamegeek.com/applications and `Authorization: Bearer <token>` since ~fall 2025; 401 otherwise; approval can take a week+; tokens may expire (Dec 2025 reports). — [bgg-ts-client README](https://cdn.jsdelivr.net/npm/bgg-ts-client@0.3.0/README.md); [Tellico mailing list Dec 2025](https://mail.kde.org/pipermail/tellico-users/2025-December/002086.html); [KDE bug 511824](https://bugs.kde.org/show_bug.cgi?id=511824)
- BGG terms prohibit use of XML API/API2 for commercial purposes without a commercial licence. — [BGG XML API Terms of Use (wiki mirror)](https://videogamegeek.com/wiki/page/XML_API_Terms_of_Use); [BGG XML API Commercial Use](https://videogamegeek.com/wiki/page/BGG_XML_API_Commercial_Use)
- Steam `IWishlistService/GetWishlist` returns wishlist items with priorities, used with standard Web API key; wishlist must be public (community sources only). — [steam-go](https://github.com/gofurry/steam-go); [IFTTT Steam wishlist](https://ifttt.com/steam/queries/get_my_wishlist)
- IGDB: 2019 forum — free under Twitch Developer Service Agreement; a Sept 2026 comparison page says commercial use requires partnership (conflicting, unverified). — [Twitch dev forum](https://discuss.dev.twitch.com/t/commercial-use-of-igdb-api/23567); [api.market blog](https://api.market/blog/sleeyax/entertainment/best-movie-apis)

### Inferences
- AniList: public GraphQL, no key for public lists; MyAnimeList API v2 needs client ID **[BK-unverified]**. Backloggd, Serializd, Rate Your Music, Untappd (API closed to new apps), The StoryGraph: no open API **[BK-unverified]**. Pinterest API needs app review; Are.na has open API for public channels **[BK-unverified]**.
- Podcasts: Apple Podcasts and Spotify have no shareable "list of shows" object readable via API; Podcast Index is free with key and catalogue use **[BK-unverified]**; the realistic model is in-app list building against Podcast Index / Apple iTunes Search API.

### Gaps
- Steam Curator lists, BGG geeklist auth specifics, AniList/MAL terms not verified.

## (e) Open catalogues and generic fallbacks

### Takeaway
Given the closures above, the robust design is in-app list building / paste-and-match against open catalogues, with OAuth reads only where the platform still allows them. Commercial-use licensing of the catalogue matters as much as the source platform.

### Cited Findings
- TMDB requires written commercial agreement (see above). — [TMDB API Terms](https://themoviedb.org:443/api-terms-of-use)
- BGG commercial licence required. — [BGG Commercial Use](https://videogamegeek.com/wiki/page/BGG_XML_API_Commercial_Use)
- Letterboxd explicitly points developers to TMDB for metadata. — [Letterboxd API Access](https://letterboxd.com/api-beta/)

### Inferences
- MusicBrainz core data CC0 (commercial OK), API 1 req/s, requires meaningful User-Agent; supports ISRC lookup **[BK-unverified]**. Wikidata: CC0, has cross-IDs (IMDb, ISBN, Spotify, Steam, BGG, MusicBrainz) — good "Rosetta stone" **[BK-unverified]**. Open Library: open data **[BK-unverified]**. ComicVine: non-commercial terms **[BK-unverified]**.
- OpenGraph/oEmbed from a shared URL gives only title/image of the list, not items **[BK-unverified]**.
- EU legality of fetching public HTML: sui generis database right (Directive 96/9/EC) protects substantial extraction; CJEU Ryanair v PR Aviation (C-30/14, 2015) held ToS can contractually restrict use of unprotected databases; GDPR applies to taste data (Art. 9 risk if inferring sensitive traits) **[BK-unverified — legal review needed]**.
- User-uploaded exports (Letterboxd CSV, Goodreads CSV, IMDb CSV, Spotify GDPR data download) are user-initiated portability and avoid platform API ToS for the app (GDPR Art. 20) **[BK-unverified]**.

### Gaps
- Primary-source verification of MusicBrainz/Wikidata/Open Library/Podcast Index/ComicVine licences not done this session; EU law points not verified with sources.
