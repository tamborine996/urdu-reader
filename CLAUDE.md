# Urdu Reader - Claude Development Notes

## Project Overview

**Purpose**: Side-by-side Urdu/English reading practice tool for language learning
**Created**: 2025-12-28
**Source**: BBC Urdu articles + fairy tales translated by Claude

## How It Works

1. Articles stored as JSON files in `articles/` folder
2. Each article has a vocabulary mapping (Urdu word → English translation)
3. JavaScript dynamically wraps vocabulary words with clickable spans
4. User clicks words to see English equivalent highlighted in same paragraph

## Key Features

### Click-to-Highlight (Paragraph-Scoped)
- Vocabulary words wrapped dynamically via `wrapVocabulary()` function
- Clicking a word highlights its translation **within the same paragraph only**
- Works bidirectionally (Urdu → English, English → Urdu)
- Visual indicator: dotted gold underline shows clickable words

### Save Vocabulary (localStorage)
- **Double-click** any word to save it to your vocabulary list
- Saved words persist across browser sessions and different articles
- Click "Saved Words" to view/manage saved vocabulary
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

## File Structure

```
Urdu-Reader/
├── index.html           # Main app
├── articles/            # JSON article files
│   ├── myanmar.json     # Myanmar Elections (~130 vocab)
│   ├── school.json      # School Punishment (~95 vocab)
│   ├── brothers.json    # Brothers Reunited (~90 vocab)
│   └── lion-mouse.json  # The Lion and the Mouse (~100 vocab)
├── CLAUDE.md            # This file
└── README.md            # User documentation
```

## Adding New Articles

### Step 1: Create JSON File

Create `articles/your-article.json`:

```json
{
  "id": "your-article",
  "category": "news",
  "title": {
    "urdu": "عنوان",
    "english": "Title"
  },
  "vocabulary": {
    "اردو لفظ": "english word",
    "حکومت": "government"
  },
  "paragraphs": [
    {
      "urdu": "اردو پیراگراف...",
      "english": "English paragraph..."
    }
  ]
}
```

### Step 2: Add to Dropdown

In `index.html`, add option to `<select id="article-selector">`:

```html
<option value="your-article">اردو نام — English Name</option>
```

### Step 3: Add to Available Articles Array

In `index.html`, update the `availableArticles` array:

```javascript
const availableArticles = ['myanmar', 'school', 'brothers', 'lion-mouse', 'your-article'];
```

### Vocabulary Guidelines

- **90-130 words** per article is ideal
- Include word variants: `بچہ` (child), `بچے` (children), `بچوں` (of children)
- Sort by length happens automatically (longer phrases matched first)
- Keys are generated from English: "civil war" → `data-word="civil-war"`

## Deployment

**GitHub Repo**: https://github.com/tamborine996/urdu-reader
**Live Site**: https://tamborine996.github.io/urdu-reader/
**Git Root**: `C:\Users\mqc20\Downloads\Projects\Urdu-Reader`

Deploy changes:
```bash
cd "C:\Users\mqc20\Downloads\Projects\Urdu-Reader"
git add -A && git commit -m "Update" && git push
```

## Architecture

### JSON-Based Storage (2025-12-30 Refactor)
- **Before**: Articles stored as inline HTML with pre-wrapped `<span>` tags
- **After**: Clean JSON with vocabulary mappings, spans generated at runtime
- **Benefits**: Scalable, easier to add articles, comprehensive vocabulary coverage

### Key Functions
- `fetchArticle(id)` - Loads and caches JSON from `articles/` folder
- `wrapVocabulary(text, vocab, isUrdu)` - Wraps matching words with spans
- `loadArticle(id)` - Renders article with dynamic vocabulary wrapping

### Visual Design
- **Theme**: "Manuscript Luminance" - dark with antique gold accents
- **Font**: Noto Nastaliq Urdu (Google Fonts)
- **Indicators**: Dotted underlines show clickable vocabulary words

## Current Articles

1. **Myanmar Elections** (میانمار الیکشن) - 22 paragraphs, ~130 vocab
2. **School Punishment** (سکول میں سزا) - 17 paragraphs, ~95 vocab
3. **Brothers Reunited** (بھائیوں کی ملاقات) - 25 paragraphs, ~90 vocab
4. **The Lion and the Mouse** (شیر اور چوہا) - 17 paragraphs, ~100 vocab (fairy tale)

## Future Enhancements (Someday)

- Audio pronunciation for vocabulary
- Spaced repetition for learned words
- Progress tracking
- Mobile-optimized view
