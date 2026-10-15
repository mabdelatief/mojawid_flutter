# Mojawid (مُجَوِّد): the product

What the app is, what the user decided, and how each screen must behave. Where things are:
- **The design:** the Claude Design export, unpacked in `design/design_handoff_mojawid/`.
  - Its `README.md` is the source for pixels, colours, type and exact copy.
  - The screenshots are in `screenshots/light/` and `screenshots/dark/`.
  - Open `design/Mojawid App.dc.html` in a browser to click through every flow.
- **The model and the analysis:** `mojawid_backend/docs/model-and-analysis.md`. That covers
  what the model hears, how mistakes are found, what the result data holds, and what the
  backend must provide.

This file is the product decisions. Where it and the design export disagree, this file wins,
and the difference is listed under "Corrections to the design".

## 1. What it is

Mojawid is a **practice app** for Quran recitation:
1. The user picks some ayat, or lets the app recognise them.
2. They listen to a professional reciter.
3. They record themselves.
4. They see where their recitation differed from the correct reading: wrong letters, wrong
   harakat, and tajweed (madd lengths, ghunnah, qalqalah, idgham, …).
5. Then they try again, and again, and see themselves improve.

The core loop is **recite → see notes → try again**.

It's for ordinary Muslims practising between lessons, not for teachers. Users range from
beginners to people preparing for an ijazah. It never replaces a licensed teacher, and says so
on every result.

**Tone.** Calm, respectful and encouraging; this is the Quran.
- Never negative: no "worse", "bad" or "failed"; no red down-arrows or falling scores.
- No game-style scores, streaks or confetti.
- Honest about uncertainty: the app can be wrong and never sounds more sure than it is.
- Kindness changes **how** a mistake is said, never **whether** it is shown.

## 2. Decisions (from the user, 2026-10-15)

**Access and money**
- Free. No sign-up, no login, no account screens, no payments.
- The backend still protects itself with API keys and rate limiting, set so they keep it
  safe without getting in a normal user's way. Details to be discussed.

**Platforms and hosting**
- One Flutter app for iOS, Android and the web.
- Backend: FastAPI, PostgreSQL, docker compose, on Linode. The speech model runs on modal.com.
  The architecture is to be discussed.

**Choosing what to recite**
- Two modes: **«سأختار الآيات»** (choose the ayat) or **«تعرّف عليها تلقائيًا»** (recognised
  automatically).
- Choosing means a surah, then "from ayah" and "to ayah". **At most 6 ayat at a time** (the
  user's figure was "6 or something"; see open question 2).
- The isti'adha and basmala options stay as in the lab app: «تبدأ بالاستعاذة», «تبدأ
  بالبسملة», both off by default.

**When the ayat are chosen**
- The user sees the text in the Uthmani script **with the tajweed colours**, and a legend.
- They pick a **reciter** and can **listen to all the chosen ayat**.
- **Tapping any word plays only that word, from the picked reciter.** Tapping the ayah number
  plays the whole ayah.
  - This holds everywhere in the app: Practice, Record, and «القراءة الصحيحة» on the result.
  - A tap always plays the **correct** word, as the reciter says it, never the user's own
    recording.
  - The user's own audio at a word plays only from the «استمع لتسجيلي» button in a mistake's
    bottom sheet (3.4).
- **Only recitations that have word-by-word audio are offered.** A reciter whose words can't
  be played on their own isn't in the list.
- Audio sources:
  - the quran.com API, everyayah;
  - the user's own Linode S3, which holds almost all the recitations, so it's a candidate
    for serving them.
- A **default reciter** in Settings, and a dropdown to switch the reciter for the current
  session.

**Recording**
- Up to **60 seconds**. The design shows the time left, and also works at 30 s.
- It stops on its own at the limit.
- The user can pick an existing audio file instead of recording.

**Results**
- **No teacher review** in this app.
- The result keeps **mistakes apart from the tajweed colours**. They used to be mixed in one
  text, and that was confusing.
- A simple top part, with everything technical under **«تحليل متقدّم»** (advanced analysis),
  collapsed by default.
- **«حاول مرة أخرى»** (try again) is the main action. Each attempt on the same ayat is
  compared with the previous one, always in an encouraging tone (section 4).
- **«نتيجة غير صحيحة؟»** (report a bad result): the user can send their audio and the result
  they were shown, with their consent, for us to investigate.

**Privacy**
- Recordings are saved **only on the user's phone**.
- The web version saves nothing.
- Nothing is kept on our side, except a report the user explicitly agreed to send. We use it
  only to investigate and improve the app, and delete it afterwards.

**Settings**
- Every reading setting the lab app offers moves to the app's Settings: speed, madd lengths,
  the meem form, raum practice. The full list and defaults are in the model doc, section 2.
- The reading is **Hafs ʿan ʿAsim via al-Shatibiyya** for now. Other readings come later and
  the user will pick one, so Settings shows them as «قريبًا» (coming soon).

## 3. Screens

The design README gives exact layout and copy; below is what each screen must do. Screenshot
paths are relative to `design/design_handoff_mojawid/screenshots/`.

### Navigation
- A bottom tab bar on the three main pages only: **تدرّب** (Practice), **تسجيلاتي** (My
  recordings, mobile only), **الإعدادات** (Settings).
- It is hidden during record, waiting, result and report, so the practice loop stays focused.

### 3.1 Practice: choose the ayat (home)
`light/01-choose-ayat.png`, `light/02-choose-ayat-legend.png`, `light/03-reciter-menu.png`,
`light/04-auto-detect.png`, `dark/01-choose-ayat.png`
- The mode switch, surah and from/to pickers (to ≥ from, at most 6 ayat), and the opening
  checkboxes.
- **«استمع للآيات»** plays all chosen ayat in order with the session's reciter. The reciter
  dropdown sits next to it.
- The text card: Uthmani text with tajweed colours, from the backend (see Corrections).
  - Every word can be tapped to play that word from the picked reciter.
  - The ayah marker ۝ plays the whole ayah.
  - The word or ayah playing is highlighted.
  - The legend lists all 7 colour classes, and dims those that don't occur in the chosen ayat.
- Auto mode replaces the pickers and text with a short explanation («اقرأ ما تشاء»).
- A privacy line under the content.
- Sticky **«ابدأ التسجيل»**.

### 3.2 Record, and the microphone
`light/05-mic-permission.png`, `light/06-mic-denied.png`, `light/07-record-ready.png`,
`light/08-recording.png`, `dark/02-record.png`
- The header shows the range, «المحاولة N», and a play button so the user can listen before
  reciting.
- The text stays visible (tajweed view).
- Elapsed time, time left (gold in the last 10 s), and a progress ring around the record
  button.
- «أو اختر ملفًا صوتيًا» picks an audio file.
- Microphone denied: an explanation, a button that opens the phone's settings, and the
  file-picker fallback.

### 3.3 Waiting
`light/09-waiting.png`, `dark/09-waiting.png`
- «نرسل تسجيلك» then «نستمع إلى تلاوتك»: one progress bar (upload, then analysis), and cancel.
- **If sending fails, the recording is kept and sent again automatically**, with a countdown
  and «أعد الإرسال الآن». The backend returns the same result for a resend (see the model doc,
  "Idempotent submit").
- This state isn't in the screenshots; the prototype has an `uploadFails` tweak for it.

### 3.4 Result
`light/10-result-attempt1.png`, `light/11-result-attempt1-bottom.png`,
`light/12-sheet-wrong-letter.png`, `light/13-sheet-madd.png`, `light/14-sheet-uncertain.png`,
`light/15-result-correct-reading.png`, `light/16-advanced-phonetic.png`,
`light/17-advanced-table.png`, `light/18-advanced-sifat.png`, `dark/03-result.png`,
`dark/04-sheet.png`, `dark/05-correct-reading.png`

**Order on the screen**
1. Header: range, attempt number.
2. Summary, worded kindly:
   - «لم نجد أخطاء», or «٣ مواضع للمراجعة، وموضعان غير مؤكَّدين»;
   - the Arabic number agreement is in the design README;
   - auto mode adds «تعرّفنا على: …» and «تطابق أيضًا: …».
3. The two views:
   - **«ملاحظاتي»** (default): plain text, no tajweed colours, mistake marks only;
   - **«القراءة الصحيحة»**: the correct text with tajweed colours, and the note that these
     colours are not notes on the user's recitation. Tapping a word plays it from the picked
     reciter.
   **Rule: tajweed colours and mistake marks never appear together.**

   In «ملاحظاتي»:
   - tapping a word without a mark plays it from the picked reciter;
   - tapping a marked word opens its bottom sheet (item 5), whose first button plays the
     reciter's word.
4. Marks in «ملاحظاتي»: six kinds, each a different shape, so colour isn't the only signal.

   | Mark | Look | Counted? |
   |---|---|---|
   | Wrong letter (most serious) | Tinted background + solid underline + number | yes |
   | Wrong haraka | Solid underline + number | yes |
   | Tajweed mistake | Wavy underline + number | yes |
   | Uncertain | Dotted underline + «؟» | **never** |
   | Stop mid-ayah | Tag «وقف» after the word | not a mistake |
   | Stop and repeat | Tag «↩» | not a mistake |
   | Corrected since last attempt | «✓» badge | — |

   What each mark is made from is in the model doc, section 4.
5. **Tapping a mark opens a bottom sheet:**
   - the kind, the place, the word;
   - expected, heard and rule; for a madd, the counts;
   - for an uncertain place, the note that the model nearly heard the correct reading and it
     isn't counted;
   - buttons:
     - «استمع للكلمة من القارئ»: the picked reciter says only that word, so the user hears it
       correctly;
     - «استمع لتسجيلي»: the user's own audio at that word, to compare. It's the only place in
       the app that plays part of the user's recording. It needs word times from the backend
       (correction 8);
     - «لم أخطئ هنا؟ أبلغنا»: opens the report with the word filled in.
6. Playback: the user's recording, and the reciter's audio for the same ayat.
7. From the second attempt: the comparison card (section 4).
8. The permanent disclaimer: «الملاحظات الآلية قد تخطئ ولا تغني عن المعلّم المجاز».
9. **«تحليل متقدّم»**, collapsed, three tabs:
   - **المقارنة الصوتية:** the expected and heard phonetic script per ayah, differences
     highlighted;
   - **جدول المواضع:** every place as a card (place, type, change, rule, expected, heard, madd
     count);
   - **صفات الحروف:** one row per letter, differing attributes highlighted, the letter column
     fixed while scrolling sideways.
10. «تلاوة جديدة», «نتيجة غير صحيحة؟».
11. Sticky: **«حاول مرة أخرى»**. When there are mistakes, also «تدرّب على الآية N»: practise
    just that ayah, which starts a new run.

### 3.5 Report a bad result
`light/23-report.png`, `light/24-report-consent.png`, `light/25-report-sent.png`
- **Optional details:**
  - a category: «أشار إلى خطأ لم أقع فيه», «لم يلاحظ خطأً وقعت فيه», «تعرّف على آيات غير التي
    قرأتها», «شيء آخر»;
  - the word;
  - a note.
- **Consent card:**
  - what is sent: the recording, the result shown, the reading settings;
  - why: so the team can listen and improve the app;
  - that it is deleted after review.
- **An unticked checkbox.** «إرسال البلاغ» stays disabled until it's ticked.
- Confirmation: «وصلنا بلاغك، جزاك الله خيرًا».

### 3.6 My recordings (mobile only)
`light/26-my-recordings.png`, `light/27-practice-run.png`, `light/28-delete-all-confirm.png`,
`dark/07-my-recordings.png`
- Recordings of the same ayat are grouped as one **practice run**. Each run shows the surah and
  range, the date, the number of attempts, the last summary, and the progress («٣ ← ١ ← ٠»).
- A run opens to its attempts: play, open the result, delete one.
- «واصل التدريب على هذه الآيات», delete a run, delete all (with a confirmation).
- Stored only on the phone. On the web this tab doesn't exist; the app says the web version
  keeps nothing.

### 3.7 Settings
`light/29-settings.png`, `light/30-settings-bottom.png`, `dark/08-settings.png`
- Reading: «حفص عن عاصم من طريق الشاطبية», and other readings «قريبًا».
- Default reciter.
- Reading choices (advanced): speed, madd lengths, meem form, with Hafs defaults from the rules
  table.
- «أتدرّب على الوقف بالرَّوم», off by default. It's the only way raum at a stop counts as
  correct; the teachers' ruling is in the model doc, section 2.
- Appearance: light, dark, and system.
- Interface language: Arabic; English «قريبًا».
- Privacy in three short lines; the disclaimer; the version.

## 4. Practising again: the attempt comparison

`light/19-result-attempt2.png`, `light/20-attempt2-progress.png`,
`light/21-result-attempt3-clear.png`, `light/22-attempt3-progress.png`,
`dark/06-attempt2-progress.png`

- «حاول مرة أخرى» goes straight to Record with the same ayat, reciter and settings. Nothing has
  to be chosen again.
- Attempts on the same ayat form a **run**: attempt 1, 2, 3…
- From attempt 2, the result shows what changed since the previous attempt:
  - **corrected places**: «أحسنت، صحّحت هذا الموضع»;
  - **places still to work on**: «ما زال يحتاج إلى مراجعة», and new ones «موضع جديد للمراجعة»;
  - **one chip per attempt** in the run; tapping a chip opens that attempt.
- **Compare real mistakes only.** Uncertain places count in neither direction. A place is the
  same place when its ayah, word and kind match.
- **Wording by outcome** (exact copy in the design README):

  | Outcome | Says |
  |---|---|
  | First attempt, no mistakes | «بارك الله فيك. ثبّت هذه الآيات بقراءتها مرة أخرى.» |
  | First attempt, mistakes | «اضغط على أي موضع لترى ما سمعناه.» |
  | Fewer mistakes | «أحسنت، صحّحت موضعًا. واصل على البقية.» |
  | None, after mistakes | «أحسنت، صحّحت كل المواضع هذه المرة.» |
  | None again | «أحسنت، تلاوة ثابتة مرة أخرى.» |
  | Same | «أنت على الطريق الصحيح، جرّب مرة أخرى.» |
  | More | «واصل، ركّز على هذه المواضع.» |

- On mobile a run is saved on the phone. On the web it lasts while the page is open.

## 5. Corrections to the design

These are things the design export assumes that the product or the backend does differently.
Each is quoted from `design/design_handoff_mojawid/README.md`.

1. **Tajweed colours: from our backend, not alquran.cloud.** The design README says the
   prototype's colours "come from the alquran.cloud `quran-tajweed` edition" and the raa
   tafkheem "needs a reliable source and scholarly review".
   - Our backend already computes all 7 classes, raa tafkheem included, from the same phonetic
     reference the recitation is checked against. They are checked on all 6,236 ayat and
     follow the teachers' rulings.
   - The app takes the spans from the API.
   - The class names map 1:1 to the design tokens (model doc, section 2).
2. **Privacy wording.** The design README flags: "the brief says recordings never leave the
   phone, yet audio is sent for analysis".
   - Policy: the recording is sent only to be analysed. It is analysed, then discarded, and
     never stored. Only a consented report is kept, and it's deleted after review.
   - The privacy copy («تبقى تسجيلاتك على هاتفك…») must say this truthfully. Final wording:
     open question 1.
3. **Word audio from the picked reciter.** The prototype plays every word in one voice
   (`audio.qurancdn.com/wbw`), whatever reciter is picked. The decision: only reciters with
   their own word audio, or word timestamps into their ayah audio, are offered.
4. **The sifat table uses the model's ten attributes.** The prototype's columns include
   «الاستعلاء / الاستفال» and «التفخيم». The model has no isti'la output. It has ten:
   - الهمس/الجهر، الشدة/الرخاوة، التفخيم/الترقيق، الإطباق، الصفير، القلقلة، التكرار، التفشي،
     الاستطالة، الغنة;
   - the values are listed in the model doc, section 1.
   The table shows those, at least the ones a letter can differ on.
5. **Ghunnah is never a number.** The prototype's sample says «غنّة: المتوقَّع ٢ · المسموع ١
   تقريبًا».
   - The model writes a held ghunnah as a run of 3–4 letters, not as harakat, so a number
     misleads.
   - The backend names it as a state: «ناقصة», «زائدة», «غير موجودة», «أُظهر الحرف بلا إخفاء».
   - Madd keeps its counts.
6. **The rule line in the bottom sheet.** The prototype's sample explains the makhraj and the
   grammar (e.g. «مخرج القاف من أقصى اللسان…»).
   - The backend gives the tajweed rule's name for tajweed mistakes, and the expected and
     heard letter or haraka for the others.
   - It has **no** explanations of makhraj or grammar. Adding them means writing that
     content, and it would need the teachers' review.
   - Until then, the sheet shows what the backend gives.
7. **All mistake data in the prototype is sample data.** The marks, phonetic strings and sifat
   tables come from the backend. Their fields and how they map to the six marks are in the
   model doc, section 4.
8. **«استمع لتسجيلي» in the bottom sheet** needs the time of each word in the user's
   recording. It is built in `mojawid_backend` (model doc, section 4, "To build here"). Word
   taps elsewhere don't need it: they play the reciter.

## 6. Open questions

1. **Privacy wording:** the exact sentences for the home screen, Settings and the report
   consent, given that audio is sent for analysis but not stored (correction 2).
2. **The ayat limit by length, not just count.** Six short ayat fit in 60 s, but one long ayah
   may not (Al-Baqarah 282 alone is well over a minute at murattal). Should the limit be "at
   most 6 ayat, and short enough to recite in 60 s"? And what does the app say when the user's
   choice is too long?
3. **Which reciters have word-by-word audio:** which of the user's S3 recitations, quran.com's
   and everyayah's have word audio or word timestamps; this sets the reciter list.
4. **The default reciter.** The prototype uses محمود خليل الحصري.
5. **Recording past 30 s:** the model was trained on clips of at most 30 s. 60 s works in
   practice but isn't measured (model doc, section 1). Decide whether the result shows a quiet
   note past 30 s, as the lab app does.
