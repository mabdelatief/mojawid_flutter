# Handoff: Mojawid (مُجَوِّد) — brand + iPhone app

## Overview
Mojawid (mojawid.com) is a free, open, account-free app for practising Quran recitation (tajweed) with an AI speech model. The user picks ayat (or lets the app detect them), records, and sees where the recitation differed from the correct reading: wrong letters, wrong harakat, tajweed rules, uncertain places, and where they stopped. The core loop is **recite → see notes → try again**, comparing attempts in one practice run.

This bundle contains the final brand (logo, symbol, colours, type, app icons, favicons) and a clickable iPhone prototype of every main flow, light and dark.

Target: **one Flutter app** for iOS, Android and web. Arabic-first, RTL, Arabic-Indic digits (٠١٢٣٤٥٦٧٨٩) everywhere. Reading: Hafs ʿan ʿAsim via al-Shatibiyya (others later).

## About the design files
The files in `design/` are **design references built in HTML** — prototypes that show the intended look and behaviour. They are not production code. Recreate them in Flutter using its own patterns (ThemeData, Directionality.rtl, Navigator/GoRouter, etc.). Open `design/Mojawid App.dc.html` in a browser to click through; open `design/Mojawid Brand Guidelines.dc.html` for the brand rules.

Reading the source: each `.dc.html` has a template (markup with inline styles and `{{ }}` holes) and a `class Component` logic block at the bottom. All data (ayat, mistakes, wording rules, colours) lives in that logic block — it is the most precise reference for behaviour.

## Fidelity
**High-fidelity.** Colours, type, spacing, copy and interactions are final for v1. Mistake data, phonetic strings and sifat tables in the prototype are **illustrative sample data** and must come from the real model.

---

## Brand

### Symbol
An eight-pointed star (rub el hizb) filled Ink teal, with a Gold star rule inset at 84% scale, and in the centre a **sukun ring + two voice arcs** (the inner arc and ring are cut out of the star; the outer arc is Gold). Vector geometry (viewBox 0 0 100 100):
- Star: `M50 7.6 L62.4 20 L80 20 L80 37.6 L92.4 50 L80 62.4 L80 80 L62.4 80 L50 92.4 L37.6 80 L20 80 L20 62.4 L7.6 50 L20 37.6 L20 20 L37.6 20 Z`, stroke 3 same colour (rounds the points slightly).
- Inner rule: same path, `scale(0.84)` around (50,50), stroke 2.6, Gold.
- Centre group `translate(24.5 24) scale(0.52)`: ring `circle cx63 cy50 r15` stroke 9 (cut-out); inner arc `M45 28.6 A28 28 0 0 0 45 71.4` stroke 7 (cut-out); outer arc `M36 17.8 A42 42 0 0 0 36 82.2` stroke 7 (Gold). Round caps.
- Ready-made files: `brand-assets/mark/*.svg` (true transparent cut-outs). Use these, don't redraw.

### Wordmarks
- Arabic: «مُجَوِّد» in **Aref Ruqaa Bold**. The shadda + kasra on the waw are **not the font's marks**: they are the brand shadda (a drawn stroke, Ink) with a Gold kasra stroke, placed over the waw. Prototype technique: render «مُجَو» + «د», and absolutely position the brand shadda (`MojawidMark` variant `g`, 0.5em square, left −0.08em, top 0.1em of the «مُجَو» span). For production, use the PNG lockups or have the lockup converted to outlined SVG in a vector tool.
- English: «mojawid» in **Jost Medium**, lowercase, with the same brand shadda (0.46em, gold kasra) over the **w**.
- Tagline: «اقرأْ وارْتَقِ» (Aref Ruqaa) / «Recite and Rise» (Jost, uppercase, letter-spacing 0.16em). Gold-text `#8A6A2A` on light. Large sizes only.
- Lockups: symbol on the reading-start side — right of Arabic, left of English. Gap ≈ 0.23 × symbol height. Symbol height = Arabic font-size, = 1.4 × English font-size.
- Clear space: ¼ of symbol height on all sides. Minimum: symbol 24 px full colour (16 px → one-colour), lockup symbol 28 px / 8 mm print.

### Colour versions
Full colour on Paper (default) · Reversed on Night ink: star Paper `#F6F1E7`, accents Pale gold `#D8B566` · One colour Ink teal · One colour White (gold becomes a cut-out).

### App icons & favicon
- iOS: `brand-assets/app-icon/ios-app-icon-1024.png` — reversed symbol at 72% on Ink teal, square (iOS masks corners).
- Android adaptive: `android-adaptive-foreground-432.(svg|png)` (symbol inside 66% safe zone), `android-adaptive-background-432.svg` (Ink teal), `android-monochrome-432.svg` (themed icon), `play-store-512.png`.
- Web: `brand-assets/favicon/favicon.svg` (adapts to prefers-color-scheme), `favicon-tile.svg`, PNGs 16/32/48/180/192/512.
- Lockups: `brand-assets/lockups/*.png` (transparent, 3×).

---

## Design tokens
Full list in `design-tokens.json`. Key values:

| Token | Light | Dark |
|---|---|---|
| bg | #F6F1E7 | #142A33 |
| surface (cards) | #FFFDF8 | #1A3540 |
| surface2 (segmented track, chips) | #EFE8DA | #21404B |
| ink | #1C2A30 | #F6F1E7 |
| muted | #5E6461 | #A9B4B3 |
| line | #E3D9C5 | #2E4E59 |
| primary (buttons) | #1F4F5C | #F6F1E7 |
| onPrimary | #F6F1E7 | #142A33 |
| accent (gold, small highlights, ayah markers) | #9A7428 (text) / #C49A3F (fills) | #D8B566 |
| mistake | #B04A22, bg rgba(176,74,34,.13) | #F2946A, bg rgba(242,148,106,.17) |
| uncertain | #5A6E7B, bg rgba(90,110,123,.13) | #A6BAC6, bg rgba(166,186,198,.15) |
| ok (corrected) | #2F6E4F, bg rgba(47,110,79,.12) | #86C9A2, bg rgba(134,201,162,.15) |

Tajweed colour classes (the 7 the app uses) — light / dark:
madd lazim 6 `#A3122A`/`#FF7088` · madd wajib muttasil `#D33B2C`/`#FF8A72` · madd jaiz (munfasil, ʿarid, leen) `#D7791B`/`#F4AA5E` · ikhfa & ghunnah `#1F8A4C`/`#66CF90` · qalqalah `#2B7BBF`/`#73B6F2` · tafkheem of raa `#22408F`/`#97A9FF` · idgham & not pronounced `#9C9C96`/`#86959A`.

**Rule:** tajweed colours and mistake marks never appear together. Mistake marks use only shape + the mistake/uncertain colours.

Type: Quran text KFGQPC Uthmanic Script HAFS (prototype: Amiri Quran), 24–25 px, line-height 2.3–2.6 (room for underlines). UI Arabic: Noto Naskh Arabic. UI Latin (future English UI): Jost. Aref Ruqaa only in the logo/tagline.
Radii: buttons 16, cards 18, segmented 14 (inner 11), icon buttons 12, sheet 24 (top), dialogs 16. Heights: primary button 56, secondary 46, icon button 40, tab bar icon 24. Screen padding 16. Minimum hit target 44.

---

## Navigation
Bottom tab bar on the three main pages only: **تدرّب** (Practice — icon is the one-colour symbol), **تسجيلاتي** (My recordings, mobile only), **الإعدادات** (Settings). Active tab colour = primary; inactive = muted; label 12 px. Hidden during record, waiting, result and report so the practice loop stays focused (those screens have their own back/close).

## Screens

### 1. Practice / choose ayat (home)
- Header: symbol 30 px + Arabic wordmark 30 px.
- Segmented control: «سأختار الآيات» | «تعرّف عليها تلقائيًا».
- Choose mode card: Surah select, «من الآية» / «إلى الآية» selects (to ≥ from), checkboxes «تبدأ بالاستعاذة», «تبدأ بالبسملة» (unchecked by default; when checked the line appears above the text, muted, centred).
- Listen card: «استمع للآيات» (primary, plays all chosen ayat in sequence) + reciter dropdown (this session only; header note «القارئ لهذه الجلسة · الافتراضي من الإعدادات»).
- Text card: Uthmani text with tajweed colours. **Every word is tappable and plays that word from the professional reciter**; tapping the ayah-end marker ۝ plays the whole ayah. Playing word/ayah gets an uncertainBg highlight. Hint: «اضغط على كلمة لتسمعها من القارئ، أو على رقم الآية لسماعها كاملة». Legend (2 columns) lists all 7 classes; classes not present in the chosen ayat are shown at 45% opacity.
- Auto mode: card with symbol, «اقرأ ما تشاء», explanation.
- Privacy line (muted, 12.5): «تبقى تسجيلاتك على هاتفك. لا تُشارَك إلا إذا أبلغت عن نتيجة ووافقت.»
- Sticky bottom: primary «ابدأ التسجيل» with a gold dot.

### 2. Record + microphone permission
- Header: back, range («الإنسان ٧–١٠»), «المحاولة N» (+ «· الإعدادات نفسها» from attempt 2), reciter play button so they can listen before the attempt.
- Text (tajweed view) scrolls in the middle.
- Bottom: elapsed `٠:١٢` (34 px bold, tabular) + «بقي ٠:٤٨» (turns gold in the last 10 s); 112 px progress ring (gold) around an 88 px record button (primary circle; gold dot when idle, paper square when recording). Hint «اضغط للتسجيل · حتى دقيقة واحدة» / «اضغط للإيقاف وإرسال التلاوة». Auto-stops at the limit. Link «أو اختر ملفًا صوتيًا» (file picker, audio/*). Limit is 60 s in the prototype; design also works for 30 s.
- First record: system mic prompt. Purpose string: «لتسجيل تلاوتك والاستماع إليها. يبقى التسجيل على هاتفك.»
- Denied screen: «نحتاج إلى الميكروفون لسماع تلاوتك», explanation, path «الإعدادات ← مُجَوِّد ← الميكروفون», primary «فتح إعدادات الهاتف» (open app settings), secondary «اختر ملفًا صوتيًا بدلًا من ذلك».

### 3. Waiting
Symbol 84 px, title «نرسل تسجيلك» → «نستمع إلى تلاوتك», one progress bar (upload = first half, analysis = second half), two steps with ✓, «يستغرق عادةً بضع ثوانٍ», «إلغاء» (returns to record). Failure: «لم يصل التسجيل بعد» + «تسجيلك محفوظ على هاتفك، وسنرسله مرة أخرى تلقائيًا بعد ٥ ثوانٍ» with countdown and «أعد الإرسال الآن». Keep the recording and retry automatically.
⚠ Privacy copy: the brief says recordings never leave the phone, yet audio is sent for analysis. Confirm the server policy and adjust wording.

### 4. Result (most important)
Top to bottom:
1. Header: close (×), range, «المحاولة N» (+ «· محاولة سابقة» when viewing an older one).
2. Summary card: one kind line — «لم نجد أخطاء» or e.g. «٣ مواضع للمراجعة، وموضعان غير مؤكَّدين» (Arabic number agreement: 1 «موضع واحد», 2 «موضعان», 3–10 «N مواضع», 11+ «N موضعًا»; uncertain: «وموضع غير مؤكَّد» / «وموضعان غير مؤكَّدين» / «وN مواضع غير مؤكَّدة»). Outcome line in primary (see wording). Auto mode: «تعرّفنا على: الإنسان ٧–١٠ · تطابق أيضًا: …».
3. Segmented toggle: **«ملاحظاتي»** (default) | **«القراءة الصحيحة»**.
   - ملاحظاتي: plain ink text, no tajweed colours. Marks:
     - Wrong letter (most serious): mistakeBg highlight + 3 px solid underline (mistake colour) + numbered badge.
     - Wrong haraka: 3 px solid underline + badge.
     - Tajweed mistake: 1.5 px **wavy** underline + badge.
     - Uncertain: 3 px **dotted** underline in uncertain colour + «؟» badge. Never counted as a mistake.
     - Stop mid-ayah: small outlined tag «وقف» after the word; stop-and-repeat: tag «↩».
     - Corrected since last attempt: small ok-coloured «✓» badge.
     - Underline offset 12 px; badges 18 px circles, superscript; numbered in reading order with Arabic digits.
     - Legend (2 columns) for the six mark types.
   - القراءة الصحيحة: the correct reference text with tajweed colours + legend; **words tappable to hear the reciter's word**, ayah marker plays the ayah. Note: «القراءة الصحيحة بألوان التجويد… هذه الألوان ليست ملاحظات على تلاوتك.»
4. Tapping a marked word opens a **bottom sheet**: kind tag, place, the word (30 px), rows المتوقَّع / سمعنا / القاعدة / الحركات (expected vs heard count, madd only). Uncertain adds: «كاد النموذج يسمع القراءة الصحيحة هنا، فقد تكون تلاوتك صحيحة. لا يُحسب هذا الموضع خطأً.» Buttons: «استمع للكلمة من القارئ» (**plays only that word**), «استمع لتسجيلي» (plays the user's audio at that spot), link «لم أخطئ هنا؟ أبلغنا» (opens report with word + category preselected).
5. Playback card: «تسجيلي» (play/pause, progress, duration) and «القارئ: …» (plays the ayat).
6. From attempt 2: «منذ المحاولة السابقة» card — corrected places (okBg rows, «أحسنت، صحّحت هذا الموضع» + word + kind), places still to work on («ما زال يحتاج إلى مراجعة» / «موضع جديد للمراجعة», tap opens sheet), and attempt chips (one per attempt: label, a dot per real mistake or a ✓, count). Tapping a chip opens that attempt. Only real mistakes count; uncertain places count in neither direction.
7. Permanent disclaimer (gold start border): «الملاحظات الآلية قد تخطئ ولا تغني عن المعلّم المجاز».
8. «تحليل متقدّم» collapsed by default, three pill tabs:
   - المقارنة الصوتية: per ayah with differences, rows المتوقَّع / المسموع, differing tokens highlighted (mistakeBg or uncertainBg).
   - جدول المواضع: on mobile each row becomes a card (place, type, change, rule, expected, heard, madd count).
   - صفات الحروف: one row per letter, columns الجهر/الهمس، الشدّة/الرخاوة، الاستعلاء/الاستفال، الإطباق/الانفتاح، القلقلة، التفخيم. Horizontal scroll with the letter column sticky; differing cells show «expected ← heard» on mistakeBg.
9. Secondary buttons: «تلاوة جديدة», «نتيجة غير صحيحة؟».
10. Sticky bottom: primary **«حاول مرة أخرى»** (always visible without scrolling) + when there are mistakes a secondary «تدرّب على الآية N» (practise just that ayah — starts a new run).

**Outcome wording** (never negative, no scores/streaks/confetti):
- First attempt, no mistakes: «بارك الله فيك. ثبّت هذه الآيات بقراءتها مرة أخرى.»
- First attempt with mistakes: «اضغط على أي موضع لترى ما سمعناه.»
- Fewer: «أحسنت، صحّحت موضعًا. واصل على البقية.» / «أحسنت، صحّحت موضعين…»
- None after mistakes: «أحسنت، صحّحت كل المواضع هذه المرة.»; none again: «أحسنت، تلاوة ثابتة مرة أخرى.»
- Same: «أنت على الطريق الصحيح، جرّب مرة أخرى.»
- More: «واصل، ركّز على هذه المواضع.»

### 5. Try again
«حاول مرة أخرى» goes straight to Record with the same ayat, reciter and options; nothing to choose again. Text and reciter audio stay available. Each attempt appends to the current run.

### 6. Report a bad result
Category radios (optional): «أشار إلى خطأ لم أقع فيه», «لم يلاحظ خطأً وقعت فيه», «تعرّف على آيات غير التي قرأتها», «شيء آخر». Tap a word (optional, chips of the recited words). Note textarea (optional). Consent card: what is sent (recording, the result shown, reading settings), why (so the team can listen and improve), retention (deleted after review). **Unticked checkbox** «أوافق على إرسال هذا التسجيل والنتيجة لهذا الغرض.»; «إرسال البلاغ» disabled (40% opacity) until ticked. Confirmation: «وصلنا بلاغك، جزاك الله خيرًا» + deletion note + «العودة إلى النتيجة».

### 7. My recordings (mobile only; web keeps nothing)
Note: «محفوظة على هذا الهاتف فقط…». Run cards: title (surah + range), date, attempts count («محاولة واحدة/محاولتان/N محاولات»), last summary, progress chips «٣ ← ١ ← ٠». Header «حذف الكل» → confirm dialog. Run detail: attempt rows (play, label · duration, summary, «النتيجة», «حذف»), primary «واصل التدريب على هذه الآيات», «حذف هذه الجلسة». Empty state text included.

### Settings
Reading: «حفص عن عاصم من طريق الشاطبية» (selected) + «قراءات أخرى» with «قريبًا» (locked). Default reciter list. Appearance: فاتح / داكن (add «تلقائي» = system). Interface language: العربية ✓, English «قريبًا». Privacy: three short lines (no account; recordings on this phone, deletable; nothing shared unless you report and agree, deleted after review). Footer: symbol, «مُجَوِّد للتدريب بين الدروس، ولا يغني عن المعلّم المجاز.», version.

---

## State (suggested)
- `session`: mode (choose/auto), surah, from, to, istiadha, basmala, reciterId (session) ; `settings`: defaultReciterId, theme, reading.
- `mic`: unknown / granted / denied.
- `recording`: isRecording, elapsedSec, limitSec (30|60).
- `upload`: phase (upload/analyse), progress, failed, retryIn; pending recording persisted until sent.
- `run`: ayat range + settings, `attempts[]` each { audioPath, durationSec, result }. `result`: detected ranges + alternatives, `marks[]` { ayah, wordIndex, kind: letter|haraka|tajweed|uncertain|waqf|repeat, expected, heard, rule, maddExpected, maddHeard, phonetic { expected, heard } , sifat[] }.
- Diff between attempt N and N−1 on real marks only (key = ayah + word + kind): fixed, remaining, new.
- Persistence: mobile stores runs + audio locally; web in memory only.

## Data & audio
- Tajweed segments in the prototype come from the alquran.cloud `quran-tajweed` edition (markup like `[q[طْ]`). Mapping: h,l,s,a,u,w,d,b → idgham/silent · g,f,c,i → ikhfa/ghunnah · q → qalqalah · p → madd jaiz · o → madd wajib · m → madd lazim · n (natural madd) uncoloured. Raa tafkheem isn't in that source; the prototype adds it by hand — **needs a reliable source and scholarly review**.
- Ayah audio (prototype): `https://everyayah.com/data/{reciterDir}/{SSS}{AAA}.mp3`.
- Word audio (prototype): `https://audio.qurancdn.com/wbw/{SSS}_{AAA}_{WWW}.mp3` (single voice). Production needs per-reciter word audio or word-level timestamps into each reciter's ayah audio so word taps match the selected reciter.

## Screenshots
`screenshots/light/` (30) and `screenshots/dark/` (9), 804×1748 (iPhone 402×874 @2x), taken from the prototype with sample data (Al-Insan 7–10, "improving" scenario: 3 mistakes → 1 → 0). Numbered in flow order: choose ayat (01–04), mic permission/denied (05–06), record (07–08), waiting (09), result attempt 1 incl. bottom sheets for each mark type and the correct-reading view (10–15), advanced analysis tabs (16–18), attempts 2–3 with progress (19–22), report flow (23–25), my recordings, practice run, delete-all (26–28), settings (29–30). The upload-failure state is not captured; see the Waiting section and the prototype's `uploadFails` tweak.

## Files
- `screenshots/` — see above.
- `design/Mojawid App.dc.html` — clickable iPhone prototype (all flows, light/dark; Tweaks: scenario improving/clean/harder, simulate upload failure). Needs `MojawidMark.dc.html`, `ios-frame.jsx`, `support.js` alongside.
- `design/Mojawid Brand Guidelines.dc.html` — brand guidelines page.
- `design/MojawidMark.dc.html` — symbol + brand shadda as SVG (variant `m` = final symbol, `x` = one-colour, `g` = brand shadda for wordmarks; other variants are discarded explorations).
- `design-tokens.json` — colours (light/dark/tajweed), fonts, type, radii, sizes.
- `brand-assets/` — mark, app icons, favicons, lockups (SVG/PNG).
