# ⚒️ WhatsUpDocMD

**Turn any raw text into a structured, ready-to-use AI prompt in Markdown — tuned for Claude or GPT.**

WhatsUpDocMD is a single-file, zero-dependency web app. Paste an idea, a rough request, or messy notes, and it forges them into a clean `.md` prompt with a role, context, instructions, and output-format scaffold — following each model's prompting conventions. No build step, no backend, no tracking. One `index.html`.

---

## 🔗 Live Demo

> **[https://me-raffyrabin.github.io/whats-app-doc-md/](https://me-raffyrabin.github.io/whats-app-doc-md/)**

Try it in 10 seconds:

1. Paste any text into the left pane
2. Pick **Claude** or **GPT**
3. Tap **Convert to .md prompt**
4. Flip between **View Raw** and **Preview**, then copy, share, or save

---

## ✨ Features

### 🎯 Model-aware prompt templates
| Mode | Structure | Accent |
|------|-----------|--------|
| **Claude** | XML-tagged sections (`<context>`, `<instructions>`, `<output_format>`) per Anthropic's prompting guidance | Gold |
| **GPT** | Pure Markdown headers (`## System`, `## User Request`, `## Instructions`, `## Output Format`) | Teal |

Switching models re-converts your text live and re-themes the whole UI.

### 👁️ View Raw / Preview toggle
The output pane has two modes, switchable with a pill toggle:

- **View Raw** — the exact `.md` source, in monospace, ready to copy
- **Preview** — the same Markdown fully *rendered*: headings, bold/italic/strikethrough, blockquotes, inline code and code blocks, bulleted/numbered/task lists (with checkboxes), links, images, tables, and dividers

The renderer is ~70 lines of built-in vanilla JS — no Markdown library — and HTML-escapes all content, so Claude's XML tags display safely as literal text.

### 🧰 Full Markdown toolbar — with a learning mode
Fifteen formatting tools plus two editing actions, all with clean, intuitive icons:

**H1 · H2 · Bold · Italic · Strikethrough · Blockquote · Inline code · Code block · Bulleted list · Numbered list · Task list · Link · Image · Table · Divider · ↩︎ Undo · 🗑 Clear**

- **Single tap** → applies the format to your selected text (or inserts a placeholder)
- **Double tap** → copies a *sample snippet* of that format to your clipboard, so you can paste it into the editor and see exactly how the syntax works
- **Undo** → steps back through recent changes (typing bursts, toolbar formatting, and clears — up to 100 states)
- **Clear** → empties the input in one tap, with a warm-red hover so it can't be mistaken — and it's always undoable

### 📤 Share anywhere
| Action | How it works |
|--------|--------------|
| **Copy** | Clipboard API |
| **Email** | `mailto:` with the prompt in the body |
| **SMS** | Cross-platform `sms:?&body=` deep link |
| **Calendar** | Generates a downloadable `.ics` event with the prompt in the description |
| **Save to Files** | Web Share API with a file payload — surfaces the native **Save to Files** sheet on iOS and Android; falls back to a `.md` download on desktop |
| **More…** | Native OS share sheet (AirDrop, Messages, Notes, etc.) |

All share actions always send the **raw Markdown**, regardless of which view mode is showing.

### 📱 Runs like a native app on iOS
- On iPhone and iPad, an **Add to Home** button appears in the header (hidden on all other devices, and hidden once installed)
- Tapping it opens a step-by-step sheet guiding you through Safari's **Share → Add to Home Screen** flow
- Installed, WhatsUpDocMD launches full screen — no browser chrome, black-translucent status bar, custom gold-on-void app icon (`apple-touch-icon.png`)

### 🎨 Design
- Void-and-gold dark theme with a Cormorant Garamond / Inter type pairing
- Responsive two-pane layout (stacks on mobile), safe-area aware
- Keyboard focus states throughout, `prefers-reduced-motion` respected

---

## 🚀 Deploy your own

WhatsUpDocMD is static — any host works. GitHub Pages is the fastest:

1. **Fork or clone** this repository
2. Keep both files at the repo root:
   ```
   index.html
   apple-touch-icon.png
   ```
3. Go to **Settings → Pages → Source**, select **Deploy from a branch**, choose `main` / root
4. Your app is live at `https://me-raffyrabin.github.io/whats-app-doc-md/`
5. Update the **Live Demo** link at the top of this README

No build tools, package managers, or servers required.

---

## 🛠️ Tech notes

- **Single file** — all CSS and JS are inline in `index.html`; the only external requests are Google Fonts
- **No dependencies, no framework** — vanilla JS, including the built-in Markdown renderer
- **Raw Markdown is the source of truth** — the preview is derived from it, and all sharing/exporting reads the raw source, never the rendered HTML
- **Undo history** snapshots before each toolbar action and at the start of each typing burst (800 ms window), capped at 100 states
- **iOS detection** covers modern iPadOS (which reports as `MacIntel`) via `maxTouchPoints`, and suppresses the install button when already running in standalone mode
- **Clipboard, Web Share, and File APIs** degrade gracefully with toast feedback when unavailable
- **Double-tap disambiguation** uses a 300 ms tap window so single-tap formatting and double-tap samples coexist on both mouse and touch

---

## 📁 Project structure

```
whats-app-doc-md
├── index.html            # The entire app (UI + converter + MD renderer)
├── apple-touch-icon.png  # 180×180 iOS home screen icon
└── README.md
```

---

## 🧭 Roadmap ideas

- [ ] Full PWA manifest + service worker for offline use and Android install prompts
- [ ] Custom prompt templates (user-defined sections)
- [ ] Redo (forward history)
- [ ] Prompt history with local storage

---

## 📄 License

MIT — use it, fork it, forge with it.
