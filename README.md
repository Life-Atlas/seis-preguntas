# Seis preguntas · Six questions

An as-is questionnaire for sports coordination, in Spanish and English.
Six questions up front, seven optional stages behind them — thirty in total.

**Live:** https://life-atlas.github.io/seis-preguntas/

## Nothing is sent anywhere

The page has no backend and makes no network calls apart from loading its two
typefaces. Answers are held in the visitor's own browser (`localStorage`) while
they type, so the tab can be closed and reopened. Getting the answers out is the
visitor's own act, and there are three ways:

- **Copy everything** — to the clipboard, to paste into email or WhatsApp.
- **Open in email** — a prefilled `mailto:` draft.
- **Download** — a plain `.txt` file.

That is deliberate. A questionnaire about who may see an athlete's data should
not quietly post that data to a third-party form service.

## Language

Spanish is the markup itself, so the page reads correctly with **no JavaScript at
all**. English lives in `data-en` attributes and is swapped in by the ES/EN
toggle; the original Spanish is stashed in `data-es` on first swap so the toggle
works in both directions. A visitor whose browser lists no Spanish preference
opens in English; a stored choice always wins.

The English glosses under the first six questions are hidden in English mode —
`html[lang="en"] .q .gloss { display: none }` — so the page never shows the same
sentence twice.

## Structure

| | |
|---|---|
| `index.html` | The whole thing. One file, no build, no dependencies. |
| Questions | `#questions textarea[id]`, each carrying `data-label` and `data-label-en` — the headings used in the exported text. |
| Stages | `<details>` elements. A restored answer force-opens the stage it came from, so recovered text is never hidden behind a collapsed summary. |

Adding a question means adding one `.q` block with a `data-label` and a
`data-label-en`. Nothing else needs editing: the script reads the questions from
the markup rather than from a parallel list that can drift out of step with it.

## Verified before publishing

Driven in a browser against the live URL, not read:

- 30 fields and 7 stages found; the toggle swaps all of them, including
  questions inside collapsed stages, and swaps back.
- Autosave writes after a 600 ms pause; a reload restores the answers and opens
  the stage they came from.
- Copy reports success; the clipboard path falls back to `execCommand` where the
  async API is unavailable or its permission is denied.
- `mailto` refuses with an explanation past ~1800 characters rather than letting
  a mail client truncate the answers silently.
- Download produces a real file, named and headed in the active language.
- Every `localStorage` read and write is wrapped — a private window or blocked
  site data degrades to "copy before you close" instead of throwing.

## Publishing

GitHub Pages, from `index.html` on `main`. No build step.
