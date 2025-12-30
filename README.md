# Urdu Reader

A side-by-side Urdu/English reading practice tool for language learners.

**Live Site**: https://tamborine996.github.io/urdu-reader/

## Features

- **Parallel Text**: Urdu paragraphs with English translations side by side
- **Click to Highlight**: Click any underlined word to highlight its translation
- **Save Vocabulary**: Double-click words to save them for later review
- **Bookmark Progress**: Mark your reading position and jump back to it
- **4 Articles**: News stories and a classic fairy tale

## How to Use

1. Select an article from the dropdown
2. Words with dotted underlines are clickable vocabulary
3. **Single-click** a word to highlight its translation in the same paragraph
4. **Double-click** to save a word to your vocabulary list
5. Click the bookmark icon to save your reading position

## Available Articles

| Article | Type | Vocabulary |
|---------|------|------------|
| Myanmar Elections | News | ~130 words |
| School Punishment | News | ~95 words |
| Brothers Reunited | News | ~90 words |
| The Lion and the Mouse | Fairy Tale | ~100 words |

## Running Locally

```bash
# Python 3
python -m http.server 3004

# Then open http://localhost:3004
```

## Adding Articles

See [CLAUDE.md](CLAUDE.md) for development documentation.
