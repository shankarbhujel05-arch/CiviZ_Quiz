# Quiz & Learning Studio

A fully **offline** quiz library, visual quiz authoring tool and practice engine.
Create subjects and nested folders, build quizzes with text / images / LaTeX math
through a visual editor, practice with timers and shuffled questions, review
detailed reports and track attempt history — all stored locally on your machine.

## Quick start (Windows / macOS / Linux)

1. Install **Node.js LTS** from https://nodejs.org (adds the `npm` command).
2. Open a terminal in this folder (VS Code: *Terminal → New Terminal*).
3. Install dependencies once:

   ```
   npm install
   ```

4. Run in development mode:

   ```
   npm run dev
   ```

   Open the printed URL (usually `http://localhost:5173`).

5. Build the offline, single-file copy:

   ```
   npm run build
   ```

   The result is `dist/index.html` — **one self-contained file** (math renderer,
   fonts and the sample image are embedded). Copy it anywhere, double-click it,
   and it runs with no server and no internet.

## Using the studio (no code needed)

- **Study Library** — create/rename/delete unlimited nested folders via buttons;
  import JSON quizzes into any folder; move, duplicate, export or delete quizzes;
  deletions always ask for confirmation and can keep contents.
- **Quiz Creator** — title, category, topic, description, difficulty, destination
  folder; add/edit/duplicate/reorder/preview questions of five types
  (multiple choice, multiple select, true/false, short answer, numerical ± tolerance).
  Attach a **LaTeX formula** (live preview + parse-error messages) and an
  **image** (auto-resized, embedded into the quiz). Unsaved changes are guarded.
- **Practice** — choose question count or random subset, shuffle questions/options,
  timed (auto-submit) or untimed, immediate or end-of-quiz feedback, explanations
  on/off. Answers persist while navigating; keyboard: `A–D`, `← →`, `Enter`, `F`.
- **Results** — score gauge, correct/incorrect/unanswered, time taken, topics to
  revise, question-by-question review, **Save Report (.txt)**, retry, return.
- **Attempt History** — per-quiz trend, best/average, review any past attempt,
  delete individual records. Records are append-only, never overwritten.
- **Settings** — default quiz configuration, LaTeX test bench, full backup
  export/restore, reset to samples, storage stats.
- **Setup Guide** (in-app) — the complete beginner manual, JSON schema docs and
  troubleshooting.

## JSON quiz format (schema v1)

Correct answers are stored **by option id, never by position**, so shuffling can't
break a key. The importer also accepts the simpler "answer = option text" style
and validates strictly (invalid files are rejected with a precise problem list).

```json
{
  "schema_version": 1,
  "title": "Calculus Practice",
  "category": "Mathematics",
  "questions": [
    {
      "id": "CALC001",
      "type": "mcq",
      "question": "What is the derivative of x^3?",
      "formula_latex": "\\frac{d}{dx}(x^3)",
      "options": [
        { "text": "3x^2", "formula_latex": "3x^{2}" },
        { "text": "x^2" },
        { "text": "3x" },
        { "text": "x^3" }
      ],
      "answer": "3x^2",
      "explanation": "Power rule: d(x^n)/dx = n·x^(n-1).",
      "difficulty": "Easy",
      "topic": "Differentiation"
    }
  ]
}
```

`type` values: `mcq`, `multi` (answer = array), `truefalse`, `short`
(`"a | b"` alternatives), `numeric` (`{ "value": 14, "tolerance": 0.01 }`).

## Where data lives

Everything is stored in the browser's **IndexedDB** (with a localStorage mirror)
on this device — no cloud, account or network call. Use **Settings → Export full
backup** regularly. Clearing browser site data clears the studio; restore from
your backup file.

## Project structure

```
src/
  App.tsx              app root: routing, persistence, mutations
  types.ts             data model + documented JSON schema (v1)
  lib/
    engine.ts          shuffle/grading/review/report/import validation
    samples.ts         sample folders + six ready-made quizzes
    storage.ts         IndexedDB persistence + folder helpers
  components/
    ui.tsx             UI kit incl. KaTeX math components
    Sidebar.tsx        navigation rail
    SetupModal.tsx     per-attempt quiz configuration
  views/
    Dashboard.tsx  Library.tsx  Editor.tsx  Runner.tsx
    Results.tsx    History.tsx  Settings.tsx  SetupGuide.tsx
```

## Dependencies

- `react`, `react-dom`, `vite`, `tailwindcss` — app shell
- `katex` — offline LaTeX math typesetting (no TeX distribution needed)
- `idb-keyval` — tiny IndexedDB persistence helper
