 # German Vocabulary Trainer

A self-contained, offline-capable German flashcard app with spaced repetition. It ships with
**7,272 words** organised into a **five-level CEFR ladder — A2 · B1 · B1+ · B2.1 · B2.2** —
each with an English meaning, one original German example sentence + English translation, and
(for verbs) the Partizip II + auxiliary.

Two sources feed the ladder:

- **DTZ (A2 + B1)** — the complete **Goethe-Institut / telc "Deutsch-Test für Zuwanderer" (DTZ)**
  A2–B1 wordlist, **2,649 words**, split into an **A2** and a **B1** deck by membership in the
  official *Goethe-Zertifikat A2 Wortliste* (**A2 = 1,076**, **B1 = 1,573**).
- **Linie 1 (B1+, B2.1, B2.2)** — the Klett *Linie 1* Kapitelwortschatz, **4,623 words**, one deck
  **per chapter**: **B1+** (769, Kap 9–16), **B2.1** "Brücke" (1,747, Kap 1–8), **B2.2** (2,107, Kap 9–16).

## Files

| File | What it is |
|------|-----------|
| **`index.html`** | The app. One self-contained file — open it or deploy it as-is. All 7,272 words are embedded inline; no server, no build step, no external requests. |
| `dtz_wortliste_2649_enriched.json` | The enriched DTZ dataset (source of truth): lemma facts + English gloss + `de` example sentence + `es` translation + `lv` (A2/B1). |
| `dtz_wortliste_2649_enriched.tsv` | Same DTZ data as tab-separated values (original columns + `de_sentence`, `en_sentence`, `freq_rank`). |
| `dtz_wortliste_2649.json` / `.tsv` | The original DTZ input list (grammar + A-range glosses only). Kept for reference. |


## Deploy (privately)

- a **private** GitHub repo + a private host (Cloudflare Pages / Netlify with access control, an
  authenticated bucket, or a personal server), **or**
- just copy `index.html` to your device and open it — everything is inline and uses relative paths
  only, so it runs from `file://`, `localhost`, or any host identically and works fully offline once
  loaded. On a phone: open it → browser menu → **Add to Home Screen** to install it as a PWA.

## App features

- **Five CEFR levels:** decks are grouped under **A2 · B1 · B1+ · B2.1 · B2.2** headings. A2 and B1
  are single DTZ decks; B1+, B2.1 and B2.2 are the Linie 1 course, one deck per chapter (K1, K2, …).
  Tap a level heading to select/deselect the whole level, or toggle individual decks.
- **New-card order:** within A2/B1, new cards are introduced **most-common-first** (by frequency,
  shuffled within each band); within the Linie levels they follow **chapter order**. Per-deck
  progress bars and due counts; pick decks to focus or skip.
- **Spaced repetition (SM-2):** daily review queue with **Again / Hard / Good / Easy** grading,
  ease factors and growing intervals; each button previews its next interval.
- **Two directions:** German→English and English→German.
- **Two modes:** self-graded flashcards (tap to flip) and multiple-choice quiz (same-part-of-speech distractors).
- **Stats & streak:** daily goal ring, 14-day history, card-maturity breakdown, hit rate, day streak.
- **Add / edit your own words** (custom entries + edits to built-in cards).
- **Audio:** tap 🔊 to hear the German word/sentence (browser speech synthesis; optional).
- **Dark / light / auto** theme.

## Persistence & backup

- All progress (each card's SM-2 state keyed by its **stable index**, review history, streak,
  settings, and your custom words/edits) is saved to **localStorage** on every change and reloaded
  on startup — it survives closing the app and persists when installed to the Home Screen.
- Progress is keyed by stable index, so reorganising the decks (e.g. the A2/B1 split) never loses
  your review history; only which decks are *selected* resets when the deck layout changes.
- **Settings → Export** downloads all your progress + custom words as a single JSON file.
- **Settings → Import** restores from that file (e.g. on a new phone). Pure client-side.

## How the data was produced

**DTZ (A2/B1):**
- **Grammar** (article, plural, Partizip II, auxiliary) comes from the official list.
- **English glosses** for A (indices 0–206) are from the source; the remaining 2,442 were authored,
  then **independently proofread for German correctness** (articles, plurals, participles,
  conjugation/separable-verb handling, gloss accuracy), which also fixed source-grammar typos
  (e.g. `Abschlüse`→`Abschlüsse`, `un`→`tun`, `Notaufnahmeen`→`Notaufnahmen`).
- **A2 vs B1 level** (`lv`) is derived by matching each headword against the official
  *Goethe-Zertifikat A2 Wortliste* (both its alphabetical list and its thematic word-groups —
  colours, numbers, professions, compass directions, etc.); headwords on the A2 list are tagged
  **A2**, the rest **B1**.
- **Frequency ranks** (`freq_rank` / `fr`, 1 = most common) come from the opensubtitles German
  frequency list (`de_50k`), matched per headword; they order new-card introduction within A2/B1.

**Linie 1 (B1+/B2.1/B2.2):**
- Parsed from the Klett *Linie 1* Kapitelwortschatz, keeping lemma facts (article, plural,
  conjugation), glossed to English, and filed under its course chapter.
- **Example sentences are original** (written fresh at each deck's level, workplace/everyday
  register) with English translations — the same treatment as the DTZ decks.
- **Partizip II + auxiliary** are filled for every verb: the free Kapitelwortschatz only prints
  full conjugation for irregular/separable verbs, so the regular ones were completed and each
  participle was cross-checked against a rule-based derivation.

The app is robust to any entry missing a field (missing lines are simply hidden).
