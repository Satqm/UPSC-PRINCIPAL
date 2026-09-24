# UPSC Principal Exam – Pro Simulator

A comprehensive, offline‑first practice & study platform for the UPSC Principal Exam (2026). Built as a single HTML file with Tailwind CSS, IndexedDB, and zero build tools.

---

## 📋 Overview

This is a full‑featured exam simulator and study companion for the UPSC Principal recruitment exam. It loads multiple question banks (English & Hindi), organises them into sets of 120 questions, and provides:

- **Practice Mode** – timed (120 min), immediate scoring (+2.5 / ‑0.83), wrong answers automatically collected.
- **Study Mode** – untimed, one question at a time, correct answer highlighted, expert advice visible.
- **Review Set** – consolidated scroll‑through view of all questions in a set with answers & explanations.
- **Unseen Tracking** – every question is marked “viewed” the moment you open it; see exactly what you haven’t touched yet.
- **Consolidated Mega Review** – merge all questions from all sets into one continuous flow, with optional filters for Unseen or Wrong questions.
- **Wrong‑Question Replay** – after a Practice test, instantly replay only the questions you got wrong, one‑by‑one with explanations.
- **Deep Analytics** – per‑set history, accuracy, total attempts, best scores.
- **Backup & Restore** – export / import all progress as JSON.
- **Digital Clock** – always visible in the header and consolidated view.
- **TV & Mobile Friendly** – typography scales with screen size; mobile layout keeps every option visible above the sticky footer.

---

## 🚀 Quick Start

1. **Clone or download** this repository.
2. Place the provided question bank JSON files in the same folder as `index.html` (see [Expected JSON Files](#-expected-json-files)).
3. Open `index.html` in any modern browser (Chrome, Edge, Firefox, Safari).
4. The app will automatically load all JSON files, organise them into sets, and show the home screen.

> **Note:** Tailwind CSS and Font Awesome are loaded from CDN. An internet connection is needed on first load, after which the browser may cache them. The app itself works fully offline.

---

## 📁 Expected JSON Files

The simulator looks for the following files in the same directory. Missing files are skipped gracefully.

| Language | Files |
|----------|-------|
| English  | `exam-questions-v1.json`, `exam-questions-v2.json`, `exam-questions-v3.json`, `exam-question-v4.json`, `exam-questions-v5.json`, `exam-questions-v6.json` |
| Mixed    | `exam-questions-exam-e-h.json` |
| Hindi    | `exam-questions-v1hindi.json`, `exam-questions-v2hindi.json`, `exam-questions-v3hindi.json`, `exam-questions-v4hindi.json` |

You can rename or add your own files by editing the arrays `ENGLISH_FILES`, `MIXED_FILES`, and `HINDI_FILES` at the top of the `<script>` section.

### Supported JSON Formats

The loader accepts three common shapes:

1. **Modern**
   ```json
   [
     {
       "question": "What is ...?",
       "options": ["A", "B", "C", "D"],
       "correctOption": 1,
       "explanation": "..."
     }
   ]
   ```

2. **Legacy CSV‑like**
   ```json
   [
     {
       "Question_Text": "What is ...?",
       "Option_A": "A",
       "Option_B": "B",
       "Option_C": "C",
       "Option_D": "D",
       "Correct_Answer": "B",
       "Brief_Explanation": "..."
     }
   ]
   ```

3. **Options as object**
   ```json
   [
     {
       "question": "What is ...?",
       "options": { "A": "A", "B": "B", "C": "C", "D": "D" },
       "correctOption": 1,
       "explanation": "..."
     }
   ]
   ```

Language can be specified per question via `language` / `Language` / `lang` field, or inferred from the filename.

If no files are found, the app generates 600 fallback questions so you can still explore the interface.

---

## 🎮 Features & Modes

### Practice Mode
- 120‑minute countdown timer.
- Immediate feedback on each answer (correct / wrong).
- +2.5 marks for correct, ‑0.83 for wrong.
- Every wrong answer is automatically saved to the **Review** tab.
- Progress is saved continuously; you can exit and resume later.
- After submission, a summary dialog offers **“Review Wrong (N)”** to replay only the incorrect questions.

### Study Mode
- No timer, no scoring.
- Correct option pre‑highlighted.
- Expert Advice (explanation) shown directly under the question.
- Manual “Expert Advice” button opens a modal with the same content.
- Useful for learning concepts.

### Review Set (Consolidated View)
- Opens a full‑screen, scroll‑through view of all questions in a set.
- Each question shows the correct answer and explanation.
- Navigate with **Prev / Next** buttons or keyboard shortcuts.
- Palette to jump to any question.
- Progress position is remembered per set.

### All Questions Mega Review
- Merges every question from every set into one long list.
- Filter chips: **Unseen** (never opened) and **Wrong** (incorrect in the current session).
- Great for a final revision sweep.

### Unseen Tracking
- Any question that appears in Practice or Study mode is marked as “viewed”.
- The **Unattempted** tab shows per‑set progress bars and a global coverage percentage.
- Click a set to open only its unseen questions in the consolidated view.

### Wrong‑Question Replay
- From the **Review** tab, click **Practice Wrong (one‑by‑one)** to open all wrong questions in the consolidated view.
- After a Practice test, the submission dialog offers a direct **Review Wrong** button.

### Analytics
- Total attempted, correct, wrong, accuracy.
- Set‑by‑set history with date, score, correct/wrong counts.

### Backup & Restore
- **Backup** downloads a JSON file containing all history, progress, wrong questions, seen markers, and consolidated positions.
- **Restore** uploads a previously exported JSON file to fully restore state.

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` `2` `3` `4` | Select option A / B / C / D (Practice mode only) |
| `0` | Previous question |
| `9` | Next question |
| `7` or `8` | Next question (when Expert Advice modal is open) |
| `Enter` / `Space` | Next question (when modal is open) |
| `Esc` | Close modal / exit consolidated view |

---

## 💾 Data Persistence

All progress is stored locally in your browser using **IndexedDB**:

- `progress` – current active session state (so you can resume after closing the tab).
- `history` – completed Practice test records.
- `wrongQuestions` – every incorrect answer, with attempt count.
- `seen` – keys of all viewed questions.
- `consolidated` – last position in each consolidated view (per set / all / filters).
- `lastSession` – snapshot of the most recent session for post‑test review.

**Data is never erased** unless you explicitly click **Reset** in the Analytics tab or **Clear All** in the Review tab.

---

## 🛠 Customisation

### Change Questions Per Set
Edit the constant:
```js
const QUESTIONS_PER_SET = 120;
```

### Change File Sources
Modify the arrays at the top of the script:
```js
const ENGLISH_FILES = [ 'your-file-1.json', 'your-file-2.json' ];
const MIXED_FILES   = [ 'mixed-file.json' ];
const HINDI_FILES   = [ 'hindi-1.json', 'hindi-2.json' ];
```

### Adjust Timer
In `initNewSession` and `startTimer`, change `timeRemaining = 120 * 60;` (seconds).

### Styling
The app uses Tailwind CSS via CDN and a small set of custom classes in the `<style>` block. You can override colours, spacing, and fonts there.

---

## 🧰 Tech Stack

- **HTML5 / CSS3 / Vanilla JavaScript** – no framework, no build step.
- **Tailwind CSS** (CDN) – utility‑first styling.
- **Font Awesome 6** (CDN) – icons.
- **IndexedDB** – client‑side database for persistence.
- **Web Audio API** – subtle sound effects for correct/wrong answers.

---

## 📱 Responsive Design

- Mobile‑first layout with a sticky footer that never hides the last option.
- Scalable typography using `rem` and media queries for large screens (up to 3000px wide) — ideal for TVs and projectors.
- Touch‑friendly buttons (min 3.5rem height) and ample spacing.
- Dark theme with high contrast for reduced eye strain.

---

## 🤝 Contributing

This is a self‑contained educational tool. If you have question banks to add, simply drop them in the folder and update the file lists. For bug reports or feature requests, please open an issue.

---

## 📄 License

This project is provided for personal study use. The question content belongs to its respective owners. The code is free to use and modify.

---

## 🙏 Acknowledgements

- UPSC aspirants and educators and of course gemini notebook created by satyam who curated the question banks.
- Open‑source communities behind Tailwind CSS, Font Awesome, and IndexedDB.

---

**Happy studying — and good luck with your exam!** 🎓
