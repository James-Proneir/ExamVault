# 🧬 Exam Vault

A mobile-first, single-file study app for NEET preparation. Store important questions and notes, practice them one by one, or take timed tests. Everything is saved locally in your browser.

- **Stack:** plain HTML + CSS + JavaScript (`<script type="module">`), no build step, no framework
- **Storage:** IndexedDB (private to your browser, works offline except for LaTeX)
- **Theme:** follows your system light/dark mode

---

## Quick start

1. Open `neet-vault.html` in a browser (or use the hosted link).
2. Go to **➕ Add** and add a few questions (or import JSON).
3. Use **📖 Practice** or **⏱️ Test** to study.

To host it yourself, upload the single HTML file anywhere (GitHub Pages, Netlify, your own server). Nothing else is needed.

---

## Features

### 📖 Practice
- Filter by subjects and chapters (select none = all).
- Question types to include: all, unattempted, last-wrong, or starred ★.
- One question at a time, checked instantly with explanation.
- Skip, star, and exit anytime.

### ⏱️ Test
- Choose subjects/chapters, number of questions, and time in minutes.
- Reverse countdown timer; auto-submits when time runs out.
- Question palette, mark-for-review, clear answer.
- Answers and explanations are shown only after submit.
- NEET marking: **+4 correct, −1 wrong** (can be switched off).
- Result screen with score, time used, chapter analysis, and an All / Wrong / Skipped review filter.
- **Retry all** or **Retry wrong & skipped** from any result.

### 🏆 Test history
- More → Test history lists past tests (up to 50), and the Test tab shows the latest 10.
- Tap a test to reopen its full analysis or re-attempt it.
- Tests taken before the re-attempt update only show date and score.

### ➕ Add questions
- **Form:** subject, chapter, question text, optional image, 4 options (up to 6 via import), correct answer, explanation, and a live preview.
- **JSON import:** paste JSON or load a `.json` file.
- Duplicates are skipped on import.

### 📝 Notes
- Save notes you learned or tend to forget, with a tag: **Important**, **Forgot**, or **New**.
- Optional image per note.
- Filter by subject, then by chapter, plus text search.

### 📊 More
- **Progress:** overall accuracy, accuracy per subject, weak chapters.
- **Question bank:** search, edit, delete.
- **Subjects & chapters:** add or delete (deleting also removes its questions and notes).
- **Backup:** export all questions and notes as JSON.

---

## LaTeX support

Math renders in questions, options, explanations, and notes using [MathJax](https://www.mathjax.org/) (loaded from a CDN, so you need internet for it).

| Type | Syntax |
|---|---|
| Inline | `$v = u + at$` or `\( ... \)` |
| Display | `$$\frac{a}{b}$$` or `\[ ... \]` |

In JSON, remember to escape backslashes: `"$\\frac{1}{2}mv^2$"`.

---

## JSON format

```json
{
  "subject": "Physics",
  "chapter": "Kinematics",
  "questions": [
    {
      "subject": "Physics",
      "chapter": "Kinematics",
      "q": "A body has $v = u + at$. Find v if u=2, a=3, t=4.",
      "image": "data:image/jpeg;base64,...",
      "options": ["10", "12", "14", "16"],
      "answer": 3,
      "explanation": "v = 2 + 3 × 4 = 14"
    }
  ],
  "notes": [
    {
      "subject": "Biology",
      "chapter": "Cell",
      "title": "Mitochondria",
      "body": "Powerhouse of the cell...",
      "tag": "Forgot",
      "image": ""
    }
  ]
}
```

**Rules**

| Field | Notes |
|---|---|
| `subject`, `chapter` | Required per item, or set once at the top level as defaults |
| `q` | Question text (LaTeX allowed). Needed unless `image` is given |
| `options` | 2 to 6 strings |
| `answer` | **1-based** number (`1` = first option) or a letter `"A"`–`"F"` |
| `explanation` | Optional |
| `image` | Optional, must be a data URI (`data:image/...`) |
| `tag` (notes) | `Important`, `Forgot`, or `New` |

A plain array of question objects also works.

**Tip:** paste your questions into any AI chatbot and ask it to convert them into this format.

---

## Backup and restore

Your data exists only in this browser, so clearing site data or switching devices loses it.

1. **More → Backup / export → Generate backup → Copy**, and save it in a file or note.
2. To restore, go to **Add → JSON import**, paste it, and tap **Import**.

Backups contain questions and notes. Practice stats and test history are not included.

---

## Known limits

- **Images:** upload them through the form, or embed them as data URIs in JSON. Remote image URLs are blocked on hosted pages. Uploaded images are resized to at most 900 px.
- **LaTeX** needs an internet connection to load MathJax.
- **Delete buttons** ask "Sure?" and need a second tap, because browser pop-ups may be blocked.
- **Storage is per browser.** A different browser or a private window starts empty.
- An active test is lost if you reload the page mid-test.

---

## Project structure

```
neet-vault.html   # the entire app (HTML + CSS + JS)
README.md
```

Data lives in IndexedDB (database `neetvault`, key `db`) with this shape:

```js
{
  subjects:  [{ id, name, chapters: [] }],
  questions: [{ id, subject, chapter, q, img, o: [], ans, exp, star, st: { a, w, last } }],
  notes:     [{ id, subject, chapter, title, body, tag, img, at }],
  tests:     [{ id, at, c, w, u, score, max, used, n, l, a, neg, dur }]
}
```

---

## Ideas for later

- Shuffle options toggle
- Spaced repetition for notes
- Per-question time tracking
- Resume an interrupted test
