# Claude Code Skills

A collection of custom slash commands for [Claude Code](https://claude.ai/code) — drop any skill into your project and invoke it with `/skill-name`.

[עברית 🇮🇱](./README.he.md)

<br/>

[![Website](https://img.shields.io/badge/roeeaizman.com-000000?style=for-the-badge&logo=About.me&logoColor=white)](https://roeeaizman.com/#guides)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/roeea)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/roeeaiautomation/)
[![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@roeea2)

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

## Installation on Claude Code

### Step 1 — Get Claude Code

If you don't have Claude Code yet, install it:

```bash
npm install -g @anthropic-ai/claude-code
```

Or download the [Claude Code desktop app](https://claude.ai/code).

### Step 2 — Add the skill to your project

Open a terminal inside your project folder and run:

```bash
mkdir -p .claude/commands
curl -o .claude/commands/transcribe-podcast.md \
  https://raw.githubusercontent.com/roeea2/transcribe-podcast-skill/main/transcribe-podcast/transcribe-podcast.md
```

> **Want it available in every project?** Install it globally instead:
> ```bash
> mkdir -p ~/.claude/commands
> curl -o ~/.claude/commands/transcribe-podcast.md \
>   https://raw.githubusercontent.com/roeea2/transcribe-podcast-skill/main/transcribe-podcast/transcribe-podcast.md
> ```

### Step 3 — Run it

Open Claude Code in your project directory and type:

```
/transcribe-podcast
```

Or pass the URL directly:

```
/transcribe-podcast https://www.bbc.co.uk/sounds/play/...
```

Claude will guide you through the rest — it installs [Whisper](https://github.com/openai/whisper) automatically if needed.

### Alternative — Clone the whole repo

```bash
git clone https://github.com/roeea2/transcribe-podcast-skill
cp transcribe-podcast-skill/transcribe-podcast/transcribe-podcast.md .claude/commands/
```

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
