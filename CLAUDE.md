# Urdu Reader - Claude Development Notes

## Project Overview

**Purpose**: Side-by-side Urdu/English reading practice tool for language learning
**Created**: 2025-12-28
**Source**: BBC Urdu articles translated by Claude

## How It Works

1. Fetch an Urdu article (e.g., from BBC Urdu)
2. Claude translates and structures it paragraph-by-paragraph
3. Key vocabulary words are tagged for cross-highlighting
4. User reads Urdu, clicks words to see English equivalent highlighted

## Key Features

### Click-to-Highlight (Paragraph-Scoped)
- Words wrapped in `<span class="word" data-word="key">` in both languages
- Clicking a word highlights its translation **within the same paragraph only**
- Works bidirectionally (Urdu → English, English → Urdu)

### Save Vocabulary (localStorage)
- **Double-click** any word to save it to your vocabulary list
- Saved words persist across browser sessions and different articles
- Click "📚 Saved Words" to view/manage saved vocabulary
- Storage key: `urdu-reader-vocab`

## Running the App

**Port**: 3004
**URL**: http://localhost:3004

```bash
# Start server (from project folder)
python -m http.server 3004

# Or with full path
"C:\Users\mqc20\AppData\Local\Programs\Python\Python313\python.exe" -m http.server 3004 --directory "C:\Users\mqc20\Downloads\Projects\Urdu-Reader"
```

**Note**: Port 3003 is reserved for Life Dashboard. Use 3004 for Urdu Reader.

## Files

```
Urdu-Reader/
├── index.html    # Main app (single-file, self-contained)
├── process.md    # Detailed article creation workflow
└── CLAUDE.md     # This file
```

## Adding New Articles

**Time estimate**: ~25-30 minutes per article

### Step 1: Extract Article
1. Navigate to `bbc.com/urdu` in Chrome
2. Use `mcp__claude-in-chrome__get_page_text` to extract raw text
3. Clean out navigation, "Most Read" sections, related links

### Step 2: Translate
- Claude translates paragraph-by-paragraph
- Preserve 1:1 paragraph mapping (Urdu paragraph → English paragraph)
- Natural English, not word-for-word

**Prompt pattern**:
> "Translate this BBC Urdu article paragraph by paragraph. Provide natural English translations that preserve the journalistic tone."

### Step 3: Tag Vocabulary (80-120 words)
Prioritize:
- High-frequency news words: حکومت (government), انتخابات (elections)
- Numbers and dates
- Proper nouns (countries, organizations)
- Words appearing multiple times

### Step 4: Build HTML
```html
<div class="paragraph-pair">
    <div class="urdu">[text with <span class="word" data-word="key">...]</div>
    <div class="english">[text with <span class="word" data-word="key">...]</div>
</div>
```

**Critical**: `data-word` must match between Urdu and English for same vocabulary item.

### Quality Tips
- Don't over-tag (80-120 words is plenty)
- Tag ALL instances of repeated words
- Use hyphens for phrases: `data-word="civil-war"` for خانہ جنگی
- Test highlighting to verify pairs are correctly linked

See `process.md` for complete workflow with code snippets.

## Architecture Decisions

- **Single HTML file**: No build process, easy to edit
- **No framework**: Vanilla JS for simplicity
- **External font**: Noto Nastaliq Urdu from Google Fonts
- **Dark theme**: Easier on eyes for reading practice

## Future Enhancements (Someday)

- Multiple article support (article selector)
- Audio pronunciation for vocabulary
- Spaced repetition for learned words
- Progress tracking
- Mobile-optimized view
