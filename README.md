# Claude Code Skills

A collection of custom slash commands for [Claude Code](https://claude.ai/code) — drop any skill into your project and invoke it with `/skill-name`.

---

## What are Skills?

Skills are markdown files that extend Claude Code with custom slash commands. Each skill is a self-contained instruction set that tells Claude exactly what to do when invoked. They live in `.claude/commands/` inside your project (or `~/.claude/commands/` for global use).

```
your-project/
└── .claude/
    └── commands/
        └── transcribe-podcast.md   ← invoked as /transcribe-podcast
```

---

## Available Skills

### `/transcribe-podcast`

> Download, transcribe, save, and analyze any podcast from a URL.

**What it does:**

1. Asks for a podcast URL (or accepts it as an argument)
2. Checks for required tools — installs [Whisper](https://github.com/openai/whisper) if missing
3. Scrapes the page to find the audio file URL automatically
4. Downloads the audio
5. Transcribes using OpenAI Whisper (`base` model by default)
6. Saves everything to a named folder in your current directory:
   - `audio.mp3` — original audio
   - `transcript.md` — full transcript with metadata
   - `analysis.md` — structured analysis

**Analysis includes:** summary, key themes, key arguments, notable quotes, speaker breakdown, takeaways, and tone.

**Requirements:** Python 3, ffmpeg (`brew install ffmpeg`)

**Usage:**
```
/transcribe-podcast
/transcribe-podcast https://www.bbc.co.uk/sounds/play/...
```

---

## Installation

### Option A — Copy a single skill

```bash
mkdir -p .claude/commands
curl -o .claude/commands/transcribe-podcast.md \
  https://raw.githubusercontent.com/roeea2/Skills/main/transcribe-podcast/transcribe-podcast.md
```

### Option B — Clone the whole repo

```bash
git clone https://github.com/roeea2/Skills
cp Skills/transcribe-podcast/transcribe-podcast.md .claude/commands/
```

Then invoke with `/transcribe-podcast` inside Claude Code.

---

## Contributing

Skills follow the Claude Code command format:

```markdown
---
description: One-line description shown in /help
argument-hint: [optional args]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebFetch
---

# Skill Title

Instructions for Claude...
```

Pull requests welcome. Each skill should live in its own folder: `skill-name/skill-name.md`.

---

## License

MIT
