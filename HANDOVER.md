# LGE 2026 Manifesto Guide: handover

An independent, single-page voter guide for South Africa's local government elections on **Wednesday 4 November 2026** (polls 07:00–21:00). Information was checked up to **3 November 2026**.

## Files

- `index.html`: the whole site in one file (HTML, CSS, JS and data). It needs no build step and no server code. Fonts load from Google Fonts; everything else is inline.
- `HANDOVER.md`: this note.

## Deploy on GitHub Pages

1. Create a public repository, for example `lge2026-guide`.
2. Add `index.html` at the root of the `main` branch.
3. Go to Settings → Pages → Build and deployment, choose "Deploy from a branch", pick `main` and `/ (root)`, and save.
4. The site appears at `https://<your-username>.github.io/lge2026-guide/` within a few minutes.

The WhatsApp share message uses the page's own address automatically (`SITE_URL = location.origin + location.pathname`), so it needs no editing after deploying.

## What the page contains

Four tabs, switched by URL hash (`#guide`, `#quiz`, `#vote`, `#ask`):

1. **Manifesto guide.** A province filter and search; province notes; metro mayoral candidates; 14 party cards (alphabetical); and a table of other contesting parties with no 2026 manifesto found.
2. **Priorities quiz.** Steps: province, then 13 issues (Skip / Matters / Top priority), then 4 optional "approach" questions, then the result. The result shows the closest match (with manifesto, website and "See it in the guide" links) and a ranked "Compare with" list. A "How matching works" note sits below.
3. **Voting day.** ID rules, ballots, steps at the station, mistakes and photos, the ward lookup (ballotbox.co.za, labelled as independent), and IEC contacts. All of this comes from the IEC's "How to vote" page.
4. **Ask the guide.** A built-in, rule-based assistant (not a live AI). It matches keywords against the page's own data, so it works without sign-in and can't be steered off-topic. It refuses to recommend or judge parties.

Each party card shows a "Last checked" date (`checked` field, default `CHECKED`). A "Print or save as PDF" button prints the selected province. A "How parties were chosen" note sits above the table of other parties. A Promise Tracker box appears with the closed banner; it holds only the method until results are declared.

There is also a footer with the disclaimer ("Why I made this"), Report an error (lge2026corrections@gmail.com), Share this guide (WhatsApp text), where to ask other questions, and sources. A "Voting has closed" banner appears automatically from 19:00 UTC (21:00 SAST) on 4 Nov 2026, linking to the IEC results.

## Where the data lives (inside the `<script>`)

- `PARTIES`: one object per party:
  - `abbr` and `name`
  - `title`: the manifesto's title
  - `launched` and `place`
  - `footprint`: text describing where it contests
  - `confirmed`: province codes with evidence of candidates or a campaign
  - `others`: `"check"` if the party is national but unconfirmed elsewhere, `"no"` if its footprint is fully stated
  - `detail`: `"full"` or `"partial"`
  - `site`: the party website, or `null` if none was verified
  - `aliases`: regexes the assistant uses to recognise the party
  - `promises`: bullets shown on the card
  - `links`: the first link is used as the quiz's "Read about its manifesto" link
  - `t`: findings for the 13 quiz issues
- `t` format: `issue: [level, "evidence"]`. Level 2 is a specific commitment, 1 a general mention, 0 nothing found.
- `QUESTIONS`: the approach questions. Each option lists the party IDs whose manifestos explicitly state that approach.
- `METROS`: mayoral candidates per metro, as `[party label, candidate or null, note, party id]`.
- `OTHERS`, `NOTES` (province notes) and `SOURCES`.

**Scoring:** each issue's weight (1, or 2 for top priority) is multiplied by the party's level. Each answered approach question adds 4 points if the party matches. The percentage is points divided by the maximum possible. Ties are sorted alphabetically.

## Rules to keep (neutrality and accuracy)

- Every party gets the same format. List them alphabetically everywhere except quiz results, which are ordered by the user's own answers with ties alphabetical.
- No party logos or party colours. The UI uses the national flag colours plus neutral black and white.
- Every claim needs a source in `SOURCES` or in the party's `links`. Paraphrase; never paste long quotes.
- Mark a party `"partial"` when its full manifesto wasn't available, and keep the caveat text.
- The assistant must never recommend a party or judge one. Keep the `reco` and `judge` checks and the scope note.
- Don't add opinion polls, predictions or exit polls (the Municipal Electoral Act restricts publishing exit polls).
- Don't promote the page inside or at voting stations on 4 Nov (Municipal Electoral Act, section 75).

## Open items

1. **Resolved 3 Nov:** the PA's Cape Town candidate is Cheslyn Daniels, formerly reported as Cheslyn Steenberg (EWN, 26 Oct 2026).
2. **Resolved 3 Nov:** the ANC's 181 candidates in six municipalities were restored by the Constitutional Court (15 Oct) and confirmed by the IEC (19 Oct).
3. **Afrika Mayibuye and GOOD/RISE stay Partial.** Afrika Mayibuye uses the party's own launch speech (26 Sep 2026); its manifesto document was not found. GOOD/RISE uses GOOD's full Cape Town manifesto, "The GOOD Way Forward" (no public URL found; cited by imprint). RISE Mzansi has no 2026 manifesto found (the file supplied was its 2024 national manifesto), so Gauteng points come from the Johannesburg candidate's plans. MK is Full, from its 18-page People's Mandate 2026.
4. **IEC candidate search:** none found on the IEC's site as of 3 Nov; ballotbox.co.za stays, labelled independent.
5. **After results:** optionally turn the page into a promise tracker, applied equally to every party that wins seats.
6. **Languages:** English only for now.

## Usage counts

The page sends anonymous "this happened" hits to a free public counter (abacus.jasoncameron.dev, namespace `lusandanongogo-lge2026`), only on the live github.io site. Keys: `visits` and `voting-day-views` (once per browser session), `quizzes-finished`, `assistant-questions` and `shares` (each time). No answers, quiz results, typed questions, cookies or identifiers are sent. View the totals at `stats.html`. The counts are public to anyone who knows the namespace, and the free service may be unreliable.

## Known limits

- Copy protection is only a deterrent (text selection and copying are blocked; right-click is allowed). Screenshots and viewing the page source still work.
- The assistant understands English keywords only, and its answers are only as complete as the data.
- Province "confirmed" flags reflect evidence found up to 3 Nov 2026, not the IEC's full ballot list.

## Prompt for your Claude

> I'm continuing work on an independent South African local-election guide in `index.html` (see `HANDOVER.md`). Keep every rule in the "Rules to keep" section. First, deploy it on GitHub Pages from my repository. Then work through the open items, starting with confirming the Patriotic Alliance's Cape Town mayoral candidate. Cite a source for every change, and keep every party in the same format.
