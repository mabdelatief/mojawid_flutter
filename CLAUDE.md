# mojawid_flutter

The app for **Mojawid (مُجَوِّد)**: one Flutter app for iOS, Android and the web, for practising
Quran recitation. It's free, with no accounts. The user recites, sees where the recitation
differed from the correct reading, and tries again. Nothing is built yet.

## Read first

This is a frontend repo, so the `frontend-design` skill goes first (global CLAUDE.md, "Frontend
repos load their design skill first"). Then:

1. `docs/product.md`: the user's decisions, every screen, the corrections to the design export,
   and the open questions.
2. `design/design_handoff_mojawid/README.md` and `design-tokens.json`: the Claude Design
   handoff, final for v1.
   - brand, colours (light, dark, tajweed), type, radii, sizes, screens and exact copy;
   - screenshots in `screenshots/`;
   - a clickable prototype in `design/Mojawid App.dc.html`. It's HTML for reference, not code
     to port.
3. `../mojawid_backend/docs/model-and-analysis.md`: what the analysis returns, and how each
   field becomes a mark on the result screen.

## Rules

- **`docs/product.md` wins over the design export** where they differ (its "Corrections to the
  design").
  - Tajweed colours come from our API, never alquran.cloud.
  - The sifat table uses the model's ten attributes.
  - Ghunnah is a state, never a number.
- **Arabic first, right to left, Arabic-Indic digits** (٠١٢٣٤٥٦٧٨٩) everywhere. Leave room for
  an English interface later.
- **Tajweed colours and mistake marks never appear together.** «ملاحظاتي» shows plain text with
  marks; «القراءة الصحيحة» shows the colours. Marks differ by shape, not only by colour.
- **Tone: always encouraging, never negative.**
  - No "worse", "bad", "failed", no red down-arrows, no scores, streaks or confetti.
  - Honest: an uncertain place is shown as uncertain, and is never counted as a mistake or as
    "worse" between attempts.
  - The disclaimer «الملاحظات الآلية قد تخطئ ولا تغني عن المعلّم المجاز» stays on every result.
- **Privacy:**
  - recordings and practice runs are stored only on the phone;
  - the web version stores nothing;
  - audio leaves the device only to be analysed, or in a report the user ticked consent for;
  - no accounts, no login.
- **Quran text** uses the KFGQPC Uthmanic Script HAFS font. Never type or alter Quran text by
  hand; it comes from the backend.

## Project tracking (Vikunja)

- **Project:** [Mojawid](https://plan.lab.egglink.com/projects/23) (id `23`) — one
  project for the whole product. Every repo in it is a **label**, not a sub-project.
- **Label:** `MJW-APP` (id `36`) — this repo's work inside that project.
- **Use `vk`** (`home-lab/bin/vk`, on PATH) rather than raw MCP calls — it applies the
  label, picks the right bucket and resolves card numbers:
  - `vk queue` — this repo's open work, plus product-wide items that carry no label
  - `vk board` — this repo's blackboard notes
  - `vk note "<title>"` — write a note; it lands in the blackboard bucket, tagged
  - `vk ask <LABEL> "<title>"` — file against another repo; the label is never omitted
  - `vk done <#N> <sha>` — comment the result and hand the card back to whoever asked
  - `vk close <#N>` — finish a card that was handed back to you
  - `vk show <#N>` — resolve a card number, and print its comment thread
- **Sibling labels** in this product:
  - `MJW-BE` (id `35`) — mojawid_backend
- **A request and its answer are ONE card.** `vk done` posts the result as a comment and moves
  the label to whoever asked. A request crossing to a *different product* needs its own card in
  that product.
- **Buckets on kanban view `97`:** `To-Do` **96** · `Doing` **97** · `Done` **98** ·
  `blackboard` **99**.
- **Blackboard:** bucket `99` holds notes that outlive the session that wrote them — gotchas,
  decisions and their reasons. Knowledge only; a claim on work is a message to the live session
  or a card in `Doing`, never a note.
- **The `#N` on a card is a per-project index, not the task id** — `vk show N` resolves it.
- **Contracts live in git** (`docs/contracts/`), referenced from a card by commit — never pasted
  into the tracker.
