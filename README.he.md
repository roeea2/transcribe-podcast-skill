<div dir="rtl">

# כישורים ל-Claude Code

אוסף של פקודות slash מותאמות אישית עבור [Claude Code](https://claude.ai/code) — הכנס כל כישור לפרויקט שלך והפעל אותו עם `/skill-name`.

[English 🇬🇧](./README.md)

<br/>

[![אתר](https://img.shields.io/badge/roeeaizman.com-000000?style=for-the-badge&logo=About.me&logoColor=white)](https://roeeaizman.com/#guides)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/roeea)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/roeeaiautomation/)
[![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/@roeea2)

---

## מה הם כישורים?

כישורים הם קבצי markdown שמרחיבים את Claude Code עם פקודות slash מותאמות. כל כישור הוא קובץ הוראות עצמאי שאומר ל-Claude בדיוק מה לעשות כשמפעילים אותו. הם שוכנים בתיקייה `.claude/commands/` בתוך הפרויקט שלך (או `~/.claude/commands/` לשימוש גלובלי).

```
your-project/
└── .claude/
    └── commands/
        └── transcribe-podcast.md   ← מופעל בתור /transcribe-podcast
```

---

## כישורים זמינים

### `/transcribe-podcast`

> הורד, תמלל, שמור ונתח כל פודקאסט מ-URL.

**מה הוא עושה:**

1. מבקש URL של פודקאסט (או מקבל אותו כארגומנט)
2. בודק כלים נדרשים — מתקין את [Whisper](https://github.com/openai/whisper) אוטומטית אם חסר
3. סורק את הדף כדי למצוא את כתובת קובץ השמע אוטומטית
4. מוריד את השמע
5. מתמלל באמצעות OpenAI Whisper (מודל `base` כברירת מחדל)
6. שומר הכל בתיקייה נפרדת עם שם הפודקאסט בתיקיית העבודה:
   - `audio.mp3` — השמע המקורי
   - `transcript.md` — תמליל מלא עם מטא-דאטה
   - `analysis.md` — ניתוח מפורט

**הניתוח כולל:** סיכום, נושאים מרכזיים, טיעונים עיקריים, ציטוטים בולטים, פירוט דוברים, המלצות, וטון.

**דרישות:** Python 3, ffmpeg (`brew install ffmpeg`)

**שימוש:**
```
/transcribe-podcast
/transcribe-podcast https://www.bbc.co.uk/sounds/play/...
```

---

## התקנה על Claude Code

### שלב 1 — קבל את Claude Code

אם עדיין אין לך Claude Code, התקן אותו:

```bash
npm install -g @anthropic-ai/claude-code
```

או הורד את [אפליקציית Claude Code](https://claude.ai/code).

### שלב 2 — הוסף את הכישור לפרויקט שלך

פתח טרמינל בתוך תיקיית הפרויקט והרץ:

```bash
mkdir -p .claude/commands
curl -o .claude/commands/transcribe-podcast.md \
  https://raw.githubusercontent.com/roeea2/transcribe-podcast-skill/main/transcribe-podcast/transcribe-podcast.md
```

> **רוצה שיהיה זמין בכל פרויקט?** התקן אותו גלובלית:
> ```bash
> mkdir -p ~/.claude/commands
> curl -o ~/.claude/commands/transcribe-podcast.md \
>   https://raw.githubusercontent.com/roeea2/transcribe-podcast-skill/main/transcribe-podcast/transcribe-podcast.md
> ```

### שלב 3 — הפעל אותו

פתח את Claude Code בתיקיית הפרויקט שלך והקלד:

```
/transcribe-podcast
```

או העבר את ה-URL ישירות:

```
/transcribe-podcast https://www.bbc.co.uk/sounds/play/...
```

Claude ידריך אותך בשאר — הוא מתקין את [Whisper](https://github.com/openai/whisper) אוטומטית אם נדרש.

### חלופה — שכפל את כל הריפו

```bash
git clone https://github.com/roeea2/transcribe-podcast-skill
cp transcribe-podcast-skill/transcribe-podcast/transcribe-podcast.md .claude/commands/
```

---

## תרומה

כישורים עוקבים אחר פורמט הפקודות של Claude Code:

```markdown
---
description: תיאור קצר שמוצג ב-/help
argument-hint: [ארגומנטים אופציונליים]
allowed-tools:
  - Bash
  - Read
  - Write
  - WebFetch
---

# כותרת הכישור

הוראות ל-Claude...
```

Pull requests מוזמנים. כל כישור צריך לשכון בתיקייה משלו: `skill-name/skill-name.md`.

---

## רישיון

MIT

</div>
