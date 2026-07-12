# ⚒️ WhatsUpDocMD

**Turn pasted text or uploaded documents into structured, ready-to-use AI prompts and true agentic-loop prompts in Markdown.**

WhatsUpDocMD is a lightweight, privacy-minded web app for turning rough requests, notes, specifications, and reference documents into clean `.md` prompts for **Claude**, **GPT**, **Claude Code**, or **Codex**.

Paste or type text as before, or upload a **TXT, RTF, DOC, MD, or PDF** file. A built-in smart context engine analyzes the material locally, infers what the request is trying to accomplish, and uses that understanding to generate either:

- a structured regular AI prompt; or
- an implementation-oriented **agentic loop** that repeatedly inspects, acts, validates, learns from results, and corrects itself until the task is complete or reaches a stopping condition.

No account, backend, database, or tracking is required.

---

## 🔗 Live Demo

> **[https://me-raffyrabin.github.io/whats-app-doc-md/](https://me-raffyrabin.github.io/whats-app-doc-md/)**

Try it in a few steps:

1. Paste or type text, drag a supported document onto the input pane, or tap **Upload**
2. Choose **Claude** or **GPT**
3. Review the smart-context summary
4. Tap **Convert to .md prompt** for a regular prompt
5. Tap **Convert to Agentic .md Loop** for a self-correcting Claude Code or Codex workflow
6. Switch between **View Raw** and **Preview**, then copy, share, email, or save the result

> **First local-AI conversion:** the browser downloads the language model once. Download time and storage vary by device and connection; later conversions reuse the browser cache.

---

## ✨ Features

### 📄 Paste, type, drop, or upload

The original copy-and-paste editor remains fully available. Document upload now feeds extracted text into the same editor and conversion pipeline, so uploaded content can still be reviewed, edited, formatted, and converted before export.

| File type | Processing method |
|---|---|
| **TXT** | Native browser text reading |
| **MD** | Native browser text reading; Markdown source is preserved |
| **RTF** | Local RTF cleanup and text extraction |
| **DOC** | Local best-effort extraction from legacy Microsoft Word binary files |
| **PDF** | Client-side text extraction with PDF.js, loaded only when a PDF is used |

The upload control also supports drag-and-drop. The current filename and character count appear above the editor.

#### File-processing notes

- Maximum upload size: **20 MB**
- Text extraction happens in the browser
- Scanned or image-only PDFs do not contain selectable text and therefore require OCR before upload
- Legacy `.doc` is a binary format with many variations; extraction is best-effort. Saving difficult files as RTF, TXT, MD, or text-based PDF will produce more reliable results
- PDF.js is downloaded from cdnjs only when PDF support is first used. The uploaded PDF itself is processed locally and is not sent to the CDN

---

### 🧠 Hybrid local-AI smart text processing engine

Before conversion, WhatsUpDocMD analyzes the source text to infer its intended meaning rather than simply wrapping it in a fixed template.

The engine now uses two complementary layers:

1. **Local language-model interpretation** — on the first conversion, the browser downloads `Xenova/flan-t5-small` through Transformers.js. The model is cached by the browser and performs semantic interpretation on the device.
2. **Deterministic verification and fallback** — the existing rules preserve exact filenames, URLs, explicit constraints, numbered requirements, and validation wording. They also generate a complete prompt when the local model cannot load or produce valid structured output.

The merged analysis identifies:

- likely task type, such as software, research, design, writing, data, planning, or general work
- primary goal and intended final outcome
- intended goals and expected deliverables
- explicit requirements
- constraints and “do not” rules
- referenced files, URLs, attachments, or sources of truth
- testing and validation signals
- success criteria
- dependencies
- risks
- assumptions
- interpretation confidence
- an appropriate expert role and validation strategy

Before conversion, the status line shows a fast rule-based pre-analysis. When **Convert** is pressed, it shows model download or semantic-analysis progress and then reports whether the result used **local AI** or the **rule fallback**.

The source text is processed in the browser. It is not sent to a hosted language-model API. The browser only downloads the Transformers.js library and model files from the configured CDN/model host.

Both regular prompts and agentic loops use the merged structured understanding.

---

### 🎯 Model-aware regular prompts

| Mode | Structure | Accent |
|---|---|---|
| **Claude** | XML-tagged smart context, source context, instructions, and output format | Gold |
| **GPT** | Markdown smart-context analysis, user request, instructions, and output format | Teal |

A regular prompt asks the target model to understand the request and produce the requested answer or artifact. It does not, by itself, require repeated tool use or self-correction.

Switching models re-converts the current source in the active conversion mode and re-themes the interface.

---

## 🔁 Agentic Loop mode

### Agentic Loop Definition

> A loop—the **agentic loop**—is what the tool does with a prompt. Claude Code and Codex do not merely answer once. They cycle:
>
> **read the prompt → plan → call a tool → observe the result → decide the next action → repeat**
>
> Tool calls may include editing a file, running a test, grepping the codebase, reading logs, launching an application, or comparing an implementation against a reference. The loop is what allows the agent to diagnose and fix its own failing tests or incomplete work without requiring the user to re-prompt.

WhatsUpDocMD expresses this as an expanded operating cycle:

> **UNDERSTAND → INSPECT → PLAN → ACT → RUN → OBSERVE → COMPARE → CORRECT → REPEAT**

The **Convert to Agentic .md Loop** button creates a task-aware loop rather than a generic prompt wrapper.

The generated loop includes:

1. **Role and responsibility** tailored to the detected task type
2. **Agentic Loop Definition** explaining repeated tool use and self-correction
3. **Smart Task Interpretation** with goals, deliverables, requirements, constraints, references, and validation signals
4. **Original Source Request** preserved as the authoritative input
5. **Read and Understand** phase with an internal task ledger
6. **Task-specific inspection priorities**
7. **Baseline establishment** before substantial changes
8. **Dynamically generated work units** derived from the intended goals and deliverables
9. **Tool-using execution loop**
10. **Tailored example trace**
11. **Failure-handling and decision rules**
12. **Dynamic completion criteria**
13. **Stopping conditions, final validation, and evidence-backed reporting**

### What the loop changes

A regular prompt effectively says:

> Understand this request and answer it.

An agentic-loop prompt says:

> Understand the request, inspect the environment, plan the next action, use tools, observe what happened, correct failures, rerun validation, and continue until the requested outcome is verified.

Example:

```text
1. Grep the codebase → find routes/patients.ts
2. Read the file → understand the current handler
3. Edit the file → add Zod validation for dob
4. Run npm test → observe 2 failures
5. Read the failures → identify a mock missing dob
6. Edit the fixtures → add a valid dob
7. Run npm test again → all tests pass
8. Review completion criteria → verified
9. Report summary → done
```

### Model-aware loop output

| Selected mode | Task framing | Agent addressed |
|---|---|---|
| **Claude** | `<source_request>` XML tags | Claude Code |
| **GPT** | `## Source Request` Markdown section | Codex |

The output label and downloaded filename follow the active mode:

- `prompt.claude.md`
- `prompt.gpt.md`
- `loop.claude.md`
- `loop.gpt.md`

---

### 👁️ View Raw / Preview

The output pane has two modes:

- **View Raw** — exact `.md` source in monospace, ready to copy or save
- **Preview** — rendered Markdown for easier review

The built-in renderer supports:

- headings
- bold, italic, and strikethrough
- blockquotes
- inline code and fenced code blocks
- bulleted, numbered, and task lists
- links and images
- tables
- horizontal rules

All source is HTML-escaped before rendering.

---

### 🧰 Markdown toolbar and learning mode

Formatting tools:

**H1 · H2 · Bold · Italic · Strikethrough · Blockquote · Inline code · Code block · Bulleted list · Numbered list · Task list · Link · Image · Table · Divider · Undo · Clear**

- **Single tap** applies formatting to the current selection or inserts a placeholder
- **Double tap** copies a sample of the selected Markdown syntax
- **Undo** restores prior typing bursts, formatting operations, and clears
- **Clear** empties the editor and resets the current generated output; the clear remains undoable

---

### 📤 Share and export

| Action | Behavior |
|---|---|
| **Copy** | Copies raw Markdown |
| **Email** | Opens a `mailto:` draft with the generated prompt |
| **SMS** | Opens the device messaging composer |
| **Calendar** | Creates a downloadable `.ics` event |
| **Save to Files** | Uses native file sharing where supported, with download fallback |
| **More…** | Opens the operating system share sheet |

Sharing always uses raw Markdown, even when Preview is active.

---

### 📱 iOS Home Screen support

On supported iPhone and iPad browsers:

- an **Add to Home** button appears when the app is not already installed
- a guided sheet explains Safari’s **Share → Add to Home Screen** flow
- the installed app opens in standalone mode with safe-area support

---

### 🎨 Design

- dark void-and-gold Claude theme
- dark teal-accented GPT theme
- Cormorant Garamond and Inter typography
- responsive two-pane desktop layout
- stacked mobile layout
- safe-area support
- keyboard-visible focus states
- reduced-motion support
- drag-and-drop visual state
- compact smart-context status display

---

## 🚀 Deploy your own

WhatsUpDocMD remains a static application and can be hosted on GitHub Pages or any static web host.

1. Fork or clone the repository
2. Keep these files at the repository root:

   ```text
   index.html
   apple-touch-icon.png
   README.md
   ```

3. Open **Settings → Pages**
4. Select **Deploy from a branch**
5. Choose the `main` branch and repository root
6. Update the Live Demo URL in this README when needed

No build tools, package managers, database, or application server are required.

---

## 🛠️ Technical notes

- **Single application file** — UI, styles, document extraction, smart analysis, converters, Markdown preview, and sharing logic live in `index.html`
- **No framework** — vanilla HTML, CSS, and JavaScript
- **Hybrid local semantic analysis** — Transformers.js loads `Xenova/flan-t5-small` on demand for semantic interpretation, then merges its structured result with deterministic task classification, requirement detection, constraint detection, and exact-reference extraction
- **Raw Markdown is the source of truth** — Preview and sharing are derived from `rawMD`
- **Stateful conversion mode** — `convMode` tracks `prompt` or `loop`
- **Dynamic loop generation** — `buildLoop(text, model, analysis)` uses the merged local-AI analysis to create tailored work units, inspection instructions, risks, dependencies, validation strategies, and completion criteria
- **Local file processing** — TXT, MD, RTF, and DOC are processed without upload to a server
- **Lazy PDF support** — PDF.js 3.11.174 loads from cdnjs only when a PDF is selected
- **Undo history** — up to 100 snapshots, including typing bursts and formatting changes
- **Graceful browser fallbacks** — Clipboard, File, Web Share, and download capabilities provide user feedback when unavailable
- **External resources** — Google Fonts load for typography; PDF.js loads only when needed for PDF extraction; Transformers.js and the local language model download only when conversion first requests local AI

---

## 🔒 Privacy

WhatsUpDocMD does not include:

- user accounts
- analytics
- tracking
- a backend
- cloud document storage
- automatic document uploads to an AI service

Source text and extracted document text remain inside the browser session and are not submitted to a hosted LLM API. The first local-AI conversion downloads JavaScript and model assets from third-party hosting, after which the browser cache can reuse them. As with any static web app, users should review the hosting environment, browser cache behavior, and third-party resource policy before processing highly sensitive material.

---

## 📁 Project structure

```text
whats-app-doc-md
├── index.html            # UI, upload readers, smart engine, prompt/loop builders, preview, sharing
├── apple-touch-icon.png  # iOS Home Screen icon
└── README.md
```

---

## 🧭 Roadmap ideas

- [ ] DOCX support with structured paragraph extraction
- [ ] OCR support for scanned PDFs and images
- [ ] Offline packaging of PDF.js
- [ ] Full PWA manifest and service worker
- [ ] User-editable smart-analysis rules
- [ ] User-editable loop phases and stopping conditions
- [ ] Prompt and loop history with local storage
- [ ] Redo support
- [ ] Fully offline packaging of Transformers.js and the local language model
- [ ] Optional model selector for faster or more capable local models

---

## 📄 License

Released under the [MIT License — use it, fork it, forge with it.](LICENSE). © 2026 realgothamknights.

---

<p align="center">Made with ✨ and a love for forging things.</p>
