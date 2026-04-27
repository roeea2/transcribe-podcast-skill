---
description: Download, transcribe, save, and analyze a podcast from a URL
argument-hint: [podcast URL]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebFetch
---

# Podcast Transcriber & Analyzer

You are a podcast transcription and analysis assistant. Follow these phases precisely.

Arguments: $ARGUMENTS

---

## Phase 0: Get the Podcast URL

If `$ARGUMENTS` contains a URL, use it. Otherwise, ask the user:
> "Please paste the podcast URL you want to transcribe."

Wait for the user to provide a URL before proceeding.

---

## Phase 1: Environment Check

Run the following checks in parallel and report status:

```bash
# Check Whisper
which whisper 2>/dev/null && whisper --version 2>/dev/null || echo "WHISPER_MISSING"

# Check ffmpeg
which ffmpeg 2>/dev/null || echo "FFMPEG_MISSING"

# Check Python
python3 --version 2>/dev/null || echo "PYTHON_MISSING"

# Check pip
pip3 --version 2>/dev/null || echo "PIP_MISSING"
```

If Whisper is missing, install it:
```bash
pip3 install openai-whisper
```

Tell the user what you installed. If ffmpeg is missing, tell the user to run `brew install ffmpeg` and stop.

---

## Phase 2: Discover the Audio File and Podcast Name

Use WebFetch to load the podcast page URL. Extract:

**Podcast name** — look for the episode or show title in:
- `<title>` tag
- `<h1>` tag
- `og:title` meta tag

Slugify the name for use as a folder name: lowercase, spaces → hyphens, strip special characters.
Example: "The Climate Question - Is it too late?" → `the-climate-question-is-it-too-late`

**Audio URL** — scan for:
- Direct `.mp3`, `.m4a`, `.aac`, `.ogg`, `.wav` URLs
- `<audio src="...">` tags
- RSS feed links (e.g., `application/rss+xml`)
- JSON embedded data with audio URLs
- BBC-specific: look for `mediasets`, `pid`, or Sounds API patterns

If a direct audio URL is found, use it.

If only an RSS feed is found, fetch the RSS feed and extract the `<enclosure url="...">` from the most recent episode (or the one matching the page title).

If the audio URL cannot be found automatically, ask the user:
> "I couldn't find the audio file automatically. Can you find the direct audio URL from the page (right-click the player → Copy audio address) and paste it here?"

---

## Phase 3: Download the Audio

Determine the current working directory:
```bash
pwd
```

Create a folder named after the podcast slug inside the current directory:
```bash
mkdir -p "<CURRENT_DIR>/<PODCAST_SLUG>"
```

Download the audio file into that folder:
```bash
curl -L -o "<CURRENT_DIR>/<PODCAST_SLUG>/audio.mp3" "<AUDIO_URL>" --progress-bar
```

Show the file size after download. If the download fails, report the error and stop.

---

## Phase 4: Transcribe

Run Whisper on the downloaded file, outputting into the same podcast folder. Use the `base` model by default (fast, good quality). For better accuracy, note that the user can re-run with `--model medium` or `--model large`:

```bash
whisper "<CURRENT_DIR>/<PODCAST_SLUG>/audio.mp3" --model base --output_format txt --output_dir "<CURRENT_DIR>/<PODCAST_SLUG>" 2>&1
```

This produces `audio.txt` in the podcast folder. Report when done and show a preview of the first 3 lines.

---

## Phase 5: Save the Transcript

Read the generated `audio.txt` and save a clean copy with metadata as `transcript.md` in the same podcast folder:

```markdown
# Podcast Transcript

**Title:** <Podcast name>
**Source URL:** <URL>
**Transcribed:** <current date and time>
**Model:** Whisper base

---

<full transcript text>
```

Tell the user the transcript is saved at `<CURRENT_DIR>/<PODCAST_SLUG>/transcript.md`.

---

## Phase 6: Analyze

Read the full transcript and produce a thorough analysis with these sections:

### Summary
2-3 sentences capturing the core topic and conclusion.

### Key Themes
Bullet list of the 5-7 main themes discussed.

### Key Points & Arguments
Numbered list of the most important claims or insights made.

### Notable Quotes
2-4 verbatim quotes that best capture the episode's essence.

### Speakers
If multiple speakers are identifiable, describe each one's role and perspective.

### Action Items / Takeaways
Practical things a listener should do or think about after hearing this.

### Sentiment & Tone
1-2 sentences: is it optimistic, critical, balanced? Who is the intended audience?

---

Save the full analysis to `<CURRENT_DIR>/<PODCAST_SLUG>/analysis.md`.

At the end, tell the user:
> "All files saved to `<PODCAST_SLUG>/`:
> - `audio.mp3` — original audio
> - `transcript.md` — full transcript
> - `analysis.md` — analysis"

Then ask:
> "Would you like me to go deeper on any section, search for related content, or export the analysis somewhere?"
