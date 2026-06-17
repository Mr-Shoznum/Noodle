# 🍜 noodle

A self-contained, single-file exam platform inspired by Moodle quizzes. Author an exam in plain Markdown, sit it under a countdown timer, and get it marked — all from one `noodle.html` file with no build step, server, or dependencies.

---

## Demo

A short walkthrough video covering authoring an exam, sitting it, and the marking flow:

📹 **[`https://www.youtube.com/watch?v=LG0US-oRbPQ`](https://www.youtube.com/watch?v=LG0US-oRbPQ)**

If you are viewing this README on a platform that renders inline HTML (e.g. a docs site), the video can be embedded directly:

```html
<video src="docs/noodle-demo.mp4" controls width="720" poster="docs/noodle-demo-poster.png">
  Your browser does not support the video tag —
  <a href="docs/noodle-demo.mp4">download the demo video</a> instead.
</video>
```

> Drop your recording at `docs/noodle-demo.mp4` (and an optional thumbnail at `docs/noodle-demo-poster.png`). GitHub will render the link as a download; documentation sites that allow raw HTML will play it inline.

| Timestamp | What it shows |
|-----------|---------------|
| 0:00 | Writing / dropping a `.md` exam on the setup page |
| 0:30 | Navigating questions, flagging, the live timer |
| 1:10 | The four question types in action |
| 1:45 | Finishing the attempt and auto-marking |
| 2:10 | Copying the AI marking prompt |

---

## Quick start

1. Open `noodle.html` in any modern browser.
2. The editor is pre-filled with a sample exam — click **Start quiz** to try it, or **Load sample exam** to restore it at any time.
3. To build your own, write Markdown in the editor or drag and drop a `.md` file onto the drop zone, then **Validate** and **Start quiz**.

No installation, accounts, or network connection are required. Answers live in the browser session only and are discarded when you leave the attempt.

---

## Features

- **Two pages in one file** — a *setup* page for building exams and a *quiz* page for sitting them.
- **Markdown-authored exams** — write or paste exams, or drag and drop an existing `.md` file.
- **Four question types** — single-select multiple choice, multi-select multiple choice, typed (free text), and cloze (fill-in-the-blank).
- **Quiz navigation** — jump to any question from the numbered navigator, which shows answered and flagged state at a glance.
- **Flagging** — mark questions to revisit; flags show as a badge on the navigator.
- **Countdown timer** — driven by the exam's `Time=` value; turns red in the final minute and auto-submits at zero.
- **Optional shuffling** — randomise question order with `ShuffleQuestions=true`.
- **Auto-marking** — single choice, multi-select (with partial credit), and cloze (per blank, case-insensitive) are graded instantly. Typed questions are flagged for manual marking.
- **Hidden model answers** — typed questions can carry a model answer that the candidate never sees, used only when marking.
- **Copy marking prompt** — after submitting, copy a ready-made prompt (questions, marks, correct/model answers, and the candidate's responses) to paste into an AI assistant for grading as a practice exam.
- **Copy "generate an exam" prompt** — from the setup page, copy a prompt containing the full format rules so an AI can build a new exam to your spec.

---

## The `.md` exam format

An exam is a Markdown file with a header block followed by one or more questions.

### Header

Place these at the top of the file, each on its own line:

| Key | Required | Example | Notes |
|-----|----------|---------|-------|
| `Time` | yes | `**Time=00:15:30**` | Time limit as `HH:MM:SS`. |
| `Title` | yes | `**Title=Sample Exam**` | Shown in the header and breadcrumb. |
| `Description` | yes | `**Description=Mid-term assessment.**` | One-line summary. |
| `TotalMarks` | optional | `**TotalMarks=100**` | Auto-calculated from question marks if omitted. |
| `ShuffleQuestions` | optional | `**ShuffleQuestions=false**` | `true` or `false`. |

### Questions

Each question starts with `## Question N`, followed by `**Type=...**` and `**Marks=...**`, then the content.

**Single-select multiple choice** — wrap the one correct option in asterisks:

```markdown
## Question 1
**Type=multiple-choice-single**
**Marks=10**
What is the capital of Australia?
- Sydney
- *Canberra*
- Melbourne
- Perth
```

**Multi-select multiple choice** — `- [ ]` for wrong, `- [*]` for correct:

```markdown
## Question 2
**Type=multiple-choice-multi**
**Marks=15**
Select all correct statements about HTML:
- [ ] HTML is a programming language.
- [*] HTML stands for HyperText Markup Language.
- [*] It is used for structuring web pages.
- [ ] HTML cannot include CSS or JS.
```

**Typed** — free text, marked manually. Add an optional single-line `**Answer=...**` model answer (hidden from the candidate):

```markdown
## Question 3
**Type=typed**
**Marks=20**
**Answer=let is block-scoped and reassignable; const is block-scoped but cannot be reassigned.**
Explain the difference between `let` and `const` in JavaScript.
```

**Cloze** — wrap each correct answer in `[[double square brackets]]`. Marks split evenly across the blanks:

```markdown
## Question 4
**Type=cloze**
**Marks=25**
The capital of France is [[Paris]]. It is known for the [[Eiffel Tower]]. The sum of 2 + 2 is [[4]].
```

---

## How marking works

| Type | Marking |
|------|---------|
| `multiple-choice-single` | Full marks for the correct option, otherwise zero. |
| `multiple-choice-multi` | Partial credit: `(correct selected − incorrect selected) / total correct`, never below zero. |
| `cloze` | Each blank is worth an equal share of the marks; comparison is case-insensitive. |
| `typed` | Not auto-marked — flagged for manual marking, with the model answer surfaced if provided. |

After submitting, **Copy marking prompt** produces text that frames the attempt as a practice exam and asks an AI to mark it, listing every question's marks, correct/model answer, and the candidate's response.

---

## Generating an exam with AI

On the setup page, **Copy "generate an exam" prompt** copies a prompt of the form:

> Please generate an .md file exam based on the following rules: *(full format spec)*. Additionally, use the following info to generate a specific exam: *(your exam details)*.

Paste it into an AI assistant, fill in the bracketed details (subject, difficulty, number and mix of questions, marks, time limit), and paste the returned `.md` back into noodle.

---

## Project structure

```
noodle.html        # the entire app — CSS at the top, JS at the bottom
README.md          # this file
docs/
  noodle-demo.mp4         # walkthrough video (add your recording here)
  noodle-demo-poster.png  # optional video thumbnail
```

---

## Browser support & notes

- Works in any current version of Chrome, Edge, Firefox, and Safari.
- Clipboard copy uses the async Clipboard API where available, with an automatic fallback for non-secure (`file://`) contexts.
- Respects `prefers-reduced-motion` and provides visible keyboard focus.
- All state is in-memory for the session; nothing is sent anywhere or persisted.

---

## License

Use, modify, and share freely. Slurp responsibly.
# Noodle
