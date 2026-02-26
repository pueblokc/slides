# Slides Web — Enhanced by KCCS

> Web presentation mode for Markdown slides — write in Markdown, present in the browser with keyboard navigation, speaker notes, and PDF export.

![Presentation View](slides_web/docs/screenshots/presentation.png)

## What's New in This Fork

- **Browser-based slide viewer** — present Markdown slides in any browser, no terminal needed
- **Speaker notes** — add `<!-- notes: ... -->` to any slide, toggle with `N`
- **Slide thumbnails** — sidebar with quick-jump navigation
- **Paste mode** — paste Markdown directly into the browser
- **PDF export** — print-optimized styles via Ctrl+P
- **WebSocket sync** — multiple browser windows stay in sync
- **Code highlighting** — syntax-colored code blocks via Pygments
- **Fullscreen mode** — press `F` for distraction-free presenting
- **Touch support** — swipe left/right on mobile devices
- **Dark theme** — elegant dark (#0d1117) with crisp typography
- **Zero frontend dependencies** — single HTML file, no npm, no build step

## Quick Start

```bash
# Install dependencies
pip install fastapi uvicorn markdown pygments python-multipart

# Run the server
python -m uvicorn slides_web.app:app --host 127.0.0.1 --port 8509

# Open in browser
# http://localhost:8509
```

A demo presentation loads automatically. Paste your own Markdown or use the API.

## Screenshots

| Screenshot | Description |
|---|---|
| ![Presentation](slides_web/docs/screenshots/presentation.png) | Main presentation view with dark theme, progress bar, and slide counter |

## Features

- **Markdown-powered** — write slides in plain Markdown, separated by `---`
- **Dark mode** — elegant dark theme (#0d1117) with crisp typography
- **Keyboard navigation** — arrow keys, space, enter, backspace
- **Speaker notes** — add `<!-- notes: your notes here -->` to any slide
- **Slide thumbnails** — sidebar with quick-jump navigation
- **Progress bar** — thin accent bar at the top showing position
- **Slide counter** — current/total in the bottom corner
- **Paste mode** — paste Markdown directly into the browser
- **PDF export** — print-optimized styles via Ctrl+P
- **WebSocket sync** — multiple browser windows stay in sync
- **Responsive** — works on desktop, tablet, and mobile (swipe support)
- **Code highlighting** — syntax-colored code blocks via Pygments
- **Frontmatter** — set title and author in YAML frontmatter
- **Touch support** — swipe left/right on mobile devices
- **Fullscreen** — press F for distraction-free presenting
- **Zero dependencies on the frontend** — single HTML file, no npm, no build step

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `->` / `Space` / `Enter` | Next slide |
| `<-` / `Backspace` | Previous slide |
| `Home` | First slide |
| `End` | Last slide |
| `S` | Toggle slide thumbnails sidebar |
| `N` | Toggle speaker notes panel |
| `E` | Open Markdown paste editor |
| `P` | Print / Export PDF |
| `F` | Toggle fullscreen |
| `?` | Show help overlay |

## API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Web presentation UI |
| `GET` | `/api/slides` | Get parsed slides as JSON |
| `POST` | `/api/load` | Load new Markdown content |
| `GET` | `/api/export/pdf` | PDF export info |
| `WS` | `/ws` | WebSocket for view sync |

### Load Markdown via API

```bash
curl -X POST http://localhost:8509/api/load \
  -H "Content-Type: application/json" \
  -d '{"content": "# Slide 1\n\nHello\n\n---\n\n# Slide 2\n\nWorld"}'
```

### Get Slides as JSON

```bash
curl http://localhost:8509/api/slides | python -m json.tool
```

## Markdown Format

Slides are separated by `---` on its own line:

```markdown
---
title: My Presentation
author: Your Name
---

# First Slide

Content here.

---

## Second Slide

- Bullet points
- **Bold text**
- `code`

<!-- notes: Speaker notes go here -->

---

## Code Example

```python
def hello():
    print("Hello, world!")
```
```

### Speaker Notes

Add notes with an HTML comment anywhere in the slide:

```markdown
## My Slide

Content visible to audience.

<!-- notes: These notes are only visible when you press N. -->
```

## Docker

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY slides_web/ slides_web/
RUN pip install --no-cache-dir fastapi uvicorn markdown pygments python-multipart
EXPOSE 8509
CMD ["uvicorn", "slides_web.app:app", "--host", "0.0.0.0", "--port", "8509"]
```

```bash
docker build -t slides-web .
docker run -p 8509:8509 slides-web
```

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `HOST` | `127.0.0.1` | Server bind address |
| `PORT` | `8509` | Server port |

## Tech Stack

- **Backend:** FastAPI + Uvicorn
- **Markdown:** Python-Markdown with fenced_code, codehilite, tables
- **Syntax Highlighting:** Pygments (Monokai theme)
- **Frontend:** Vanilla JS, single HTML file, zero build tools
- **Real-time:** WebSocket for multi-window sync

---

<details>
<summary>Original Project README</summary>

# Slides

Slides in your terminal.

<p align="center">
  <img src="./assets/slides-1.gif?raw=true" alt="Slides Presentation" />
</p>

### Installation
[![Homebrew](https://img.shields.io/badge/dynamic/json.svg?url=https://formulae.brew.sh/api/formula/slides.json&query=$.versions.stable&label=homebrew)](https://formulae.brew.sh/formula/slides)
[![Snapcraft](https://snapcraft.io/slides/badge.svg)](https://snapcraft.io/slides)
[![AUR](https://img.shields.io/aur/version/slides?label=AUR)](https://aur.archlinux.org/packages/slides)

<details markdown="block">
<summary>Instructions</summary>

#### MacOS
```
brew install slides
```
#### Arch
```
yay -S slides
```
#### Nixpkgs (unstable)
```
nix-env -iA nixpkgs.slides
```
#### Any Linux Distro running `snapd`
```
sudo snap install slides
```
#### Go
```
go install github.com/maaslalani/slides@latest
```
From source:
```
git clone https://github.com/maaslalani/slides.git
cd slides
go install
```

You can also download a binary from the [releases](https://github.com/maaslalani/slides/releases) page.

</details>


### Usage
Create a simple markdown file that contains your slides:

````markdown
# Welcome to Slides
A terminal based presentation tool

---

## Everything is markdown
In fact, this entire presentation is a markdown file.

---

## Everything happens in your terminal
Create slides and present them without ever leaving your terminal.

---

## Code execution
```go
package main

import "fmt"

func main() {
  fmt.Println("Execute code directly inside the slides")
}
```

You can execute code inside your slides by pressing `<C-e>`,
the output of your command will be displayed at the end of the current slide.
````

Checkout the [example slides](https://github.com/maaslalani/slides/tree/main/examples).

Then, to present, run:
```
slides presentation.md
```

If given a file name, `slides` will automatically look for changes in the file and update the presentation live.

`slides` also accepts input through `stdin`:
```
curl http://example.com/slides.md | slides
```

### Navigation

Go to the next slide with any of the following key sequences:
* <kbd>space</kbd> / <kbd>right</kbd> / <kbd>down</kbd> / <kbd>enter</kbd> / <kbd>n</kbd> / <kbd>j</kbd> / <kbd>l</kbd> / <kbd>Page Down</kbd>

Go to the previous slide with any of the following key sequences:
* <kbd>left</kbd> / <kbd>up</kbd> / <kbd>p</kbd> / <kbd>h</kbd> / <kbd>k</kbd> / <kbd>N</kbd> / <kbd>Page Up</kbd>

Go to a specific slide: number + <kbd>G</kbd>

Go to the first slide: <kbd>g</kbd> <kbd>g</kbd>

Go to the last slide: <kbd>G</kbd>

### Search

Press <kbd>/</kbd>, enter your search term and press <kbd>Enter</kbd>.
Press <kbd>ctrl+n</kbd> after a search to go to the next search result.

### Code Execution

Press <kbd>ctrl+e</kbd> on a slide with a code block to execute it and display the result.

### Configuration

`slides` allows you to customize your presentation's look and feel with metadata at the top of your `slides.md`.

```yaml
---
theme: ./path/to/theme.json
author: Gopher
date: MMMM dd, YYYY
paging: Slide %d / %d
---
```

### SSH

Slides is accessible over `ssh` if hosted on a machine through the `slides serve [file]` command.

### Alternatives

* [`lookatme`](https://github.com/d0c-s4vage/lookatme)
* [`sli.dev`](https://sli.dev/)
* [`sent`](https://tools.suckless.org/sent/)
* [`presenterm`](https://github.com/mfontanini/presenterm)

### Development
See the [development documentation](./docs/development)

</details>

---

Developed by **[KCCS](https://kccsonline.com)** — [kccsonline.com](https://kccsonline.com)
