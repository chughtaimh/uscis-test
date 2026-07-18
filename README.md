# Citizenship Practice

A single-file practice app — [`citizenship-flashcards.html`](citizenship-flashcards.html) — for preparing for the U.S. naturalization (N-400) interview. Designed for an older adult, hard-of-hearing learner using a tablet: big buttons, large text, warm feedback, read-aloud everywhere, and no accounts, installs, or build steps. Open the file and practice.

## The four practice modes

### 📘 Civics
Flashcards for the deck's 103 history & government questions — the 2008 civics test (which governs N-400s filed before Oct 20, 2025), with location-specific answers for New York. Cards are shuffled; tap to reveal the answer(s), then self-mark **I got it right / I missed it / Try again later** — "try again" cards return later in the deck. Progress bar, star count, confetti on correct answers, and a final score screen with encouragement. Every card can be read aloud with the 🔊 button.

### 📖 Reading
Practices the real reading test (read a sentence aloud). Shows one of 12 official-vocabulary sentences; tap **🎤 Read it out loud** and read it. The recording is checked automatically:
- **With an OpenAI key**: audio is recorded and transcribed with Whisper.
- **Without a key**: falls back to the browser's built-in speech recognition.

Grading is lenient like the real test — 80% of the content words is a pass. Includes the official reading vocabulary list for reference. The mic permission is requested once and reused for the whole session.

### ✍️ Writing — a full simulated tablet test
A faithful, deliberately-harder simulation of the real USCIS writing test, which is administered on a tablet with a stylus. Each test:

- **20 random sentences** drawn from a 102-sentence bank built from the official USCIS writing vocabulary (M-1178) plus sentences reported verbatim from real interviews (~2/3 of each test comes from the reported-on-real-tests pool).
- **Dictation only** — the sentence is read aloud (0.85 speed) and never shown on screen while writing.
- **He must ASK for repeats, out loud.** Tapping **🎤 Ask to repeat that** only opens the microphone. He has to actually say a request — *"Could you repeat the sentence, please?"* — and an AI "officer" judges whether it understood a genuine repeat request. Understood → *"Of course."* and the sentence is re-read slower (0.7 speed). Not understood → **no repeat**, just what was heard and coaching to ask again. This trains the exact skill a hard-of-hearing applicant needs at the interview.
- **A real notepad**: he handwrites on the screen with stylus or finger — smoothed ink, palm rejection (touch is ignored once a stylus has been used), Undo, and Start-again. The paper **alternates lined / unlined** each sentence, since the real tablet may not have lines.
- **Tough AI grading**: the handwriting image goes to a vision model that transcribes it exactly (no autocorrect) and grades strictly — every word present, in order, written out in full, correctly spelled. Word swaps ("is" for "has"), missing words, and abbreviations ("US" for "United States") fail. **Capitalization, punctuation, and word spacing NEVER count** — enforced both in the prompt and by a deterministic code-level safety net (`wtSanitizeVerdict`) that overrides the model if it tries to deduct for formatting ("president" = "President", "Washington DC" = "Washington, D.C.", a comma is not a word). Digits and number words are both accepted ("50" = "fifty").
- **Honest comfort**: when a sentence fails the tough grade but a real officer would have accepted it (e.g., a spelling slip), a green banner says so.
- **Scoring**: each sentence shows the transcription, the target, and every error. Skipping counts as a miss. Final screen: percentage, **pass at 70%** (far harsher than the real test's 1-of-3), a "Worth another look" review of every missed sentence, and the reminder that the real test needs only 1 sentence out of 3.
- **Works without an API key**: grading falls back to honest self-checking against the revealed sentence (strict rules explained), and the repeat-request check falls back to browser speech recognition with keyword matching — and, if no microphone at all, an honor-system prompt that still makes him say the phrase.

### 🗣️ Speaking
15 typical interview questions (name, address, travel, oath, etc.), each read aloud, practiced by answering out loud and self-marking. Reinforces that it's always okay to say *"Could you please repeat that?"*

## AI features & the OpenAI key

The app works fully offline/keyless with the fallbacks above. Adding an OpenAI API key enables: Whisper transcription (Reading + the Writing repeat-request), AI judgment of the spoken repeat request (gpt-4o-mini), and AI handwriting grading (gpt-4o-mini by default; a Settings toggle switches to gpt-4o for messy handwriting).

**Add the key on the device**: tap **⚙️ Settings** → paste the key → Save. It is stored only in that browser's localStorage — never in the site's code. Alternative for local use: copy [`config.example.js`](config.example.js) to `config.js` (gitignored) and add a `<script src="config.js"></script>` line in the HTML head.

**Security note**: any key used by a purely static site is visible in the browser's dev tools. For personal/family use set a low usage cap on the OpenAI account; for anything more, put the key behind a small serverless proxy.

## Running it

- **Simplest**: double-click `citizenship-flashcards.html` (everything, including icons and the civics data, is baked into the one file). Note: microphone features need HTTPS or localhost in most browsers.
- **Local server**: `python3 -m http.server 8462` in this folder, then open `http://localhost:8462/citizenship-flashcards.html`. (A `.claude/launch.json` is set up for this.)
- **GitHub Pages**: the file works as-is when hosted; it's installable to the home screen (PWA meta tags + manifest included).

## Other files in this project

| File | What it is |
|---|---|
| `citizenship-flashcards.html` | The entire app (one file) |
| `config.example.js` | Optional file-based API key template (copy to `config.js`, gitignored) |
| `uscis_civics_flashcards_nyc_2026.json` | Civics question data source (NYC-specific answers) |
| `Written Exam/` | Research & study kit for the writing test: official vocabulary, 136-sentence master list, scoring-rules cheat sheet, 3-week drill plan |
| `N-400.pdf`, `N-400_Correction_Sheet.docx` | The filed application and corrections |

## Why the writing test is graded so hard

The real test asks for 1 acceptable sentence out of 3, and officers must forgive spelling, capitalization, and punctuation unless meaning is lost. This app demands 14 of 20 with perfect words. Passing here means the real test should feel easy — and the score screen always tells him so.
