 # German Vocabulary Trainer

A self-contained, offline-capable German flashcard app with spaced repetition. The app itself is
level-agnostic (more word packs, e.g. B2, can be added later). It ships with the complete
**Goethe-Institut / telc "Deutsch-Test für Zuwanderer" (DTZ)** A2–B1 vocabulary list — all
**2,649 words**, each with an English meaning and one original German example sentence + translation.

## Files

| File | What it is |
|------|-----------|
| **`index.html`** | The app. One self-contained file — open it or deploy it as-is. All 2,649 words are embedded inline; no server, no build step, no external requests. |
| `dtz_wortliste_2649_enriched.json` | The enriched dataset (source of truth): lemma facts + English gloss + `de` example sentence + `es` translation. |
| `dtz_wortliste_2649_enriched.tsv` | Same data as tab-separated values (original columns + `de_sentence`, `en_sentence`, `freq_rank`). |
| `dtz_wortliste_2649.json` / `.tsv` | The original input list (grammar + A-range glosses only). Kept for reference. |

## Deploy to GitHub Pages

1. Push this repo (already set up at `github.com/avnred666/b2-vokabeltrainer`).
2. Settings → Pages → deploy from `main` (root).
3. Once enabled the app is live at **https://avnred666.github.io/b2-vokabeltrainer/** —
   open it on your phone → browser menu → **Add to Home Screen**.

Everything is inline and uses relative paths only, so it runs from `file://`, `localhost`,
or GitHub Pages identically, and works fully offline once loaded.

## App features

- **Spaced repetition (SM-2):** daily review queue with **Again / Hard / Good / Easy** grading,
  ease factors and growing intervals; each button previews its next interval.
- **Two directions:** German→English and English→German.
- **Two modes:** self-graded flashcards (tap to flip) and multiple-choice quiz (same-part-of-speech distractors).
- **Frequency decks:** words are grouped into decks of 150 by how common they are (**Deck 1 = most
  frequent**). New cards are always introduced most-common-first (shuffled within each band), so the
  highest-utility words are learned first. Per-deck progress bars and due counts; pick decks to focus or skip.
- **Stats & streak:** daily goal ring, 14-day history, card-maturity breakdown, hit rate, day streak.
- **Add / edit your own words** (custom entries + edits to built-in cards).
- **Audio:** tap 🔊 to hear the German word/sentence (browser speech synthesis; optional).
- **Dark / light / auto** theme.

## Persistence & backup

- All progress (each card's SM-2 state keyed by its **stable index**, review history, streak,
  settings, and your custom words/edits) is saved to **localStorage** on every change and reloaded
  on startup — it survives closing the app and persists when installed to the Home Screen.
- **Settings → Export** downloads all your progress + custom words as a single JSON file.
- **Settings → Import** restores from that file (e.g. on a new phone). Pure client-side.

## How the data was produced

- **Grammar** (article, plural, Partizip II, auxiliary) comes from the official list.
- **English glosses** for A (indices 0–206) are from the source; the remaining 2,442 were authored.
- **Example sentences are original** — the source deliberately excludes its own examples for
  copyright, so every `de`/`es` sentence here was written fresh at A2–B1 level.
- Content was generated, then **independently proofread for German correctness** (articles,
  plurals, participles, conjugation/separable-verb handling, gloss accuracy, and level), which also
  fixed source-grammar typos (e.g. `Abschlüse`→`Abschlüsse`, `un`→`tun`, `Notaufnahmeen`→`Notaufnahmen`).
- **Frequency ranks** (`freq_rank` / `fr`, 1 = most common) come from the opensubtitles German
  frequency list (`de_50k`), matched per headword. They drive the frequency decks; ~97% of words
  matched, the rest (rare/administrative terms) sort into the last decks.

The app is robust to any entry missing a field (missing lines are simply hidden).
