# Urdu Reader Creation Process

A replicable workflow for creating side-by-side Urdu/English reading practice pages from BBC Urdu articles.

---

## Step 1: Navigate to BBC Urdu

1. Open Chrome browser
2. Navigate to `https://www.bbc.com/urdu`
3. Select an article of interest (news, features, etc.)

---

## Step 2: Extract Article Text

Use the browser automation to get the full article text:

```
Tool: mcp__claude-in-chrome__get_page_text
Parameter: tabId (from tabs_context_mcp)
```

This returns the raw Urdu text with title, paragraphs, and metadata.

**Clean the text**: Remove navigation elements, "Most Read" sections, related article links, and other non-article content.

---

## Step 3: Claude Translation

Have Claude translate the article paragraph-by-paragraph. Key instructions:

1. **Preserve paragraph structure** - Keep 1:1 mapping between Urdu and English paragraphs
2. **Natural translation** - Not word-for-word, but natural English that conveys meaning
3. **Context-aware** - Claude understands news context better than machine translation

**Prompt pattern**:
> "Translate this BBC Urdu article paragraph by paragraph. Provide natural English translations that preserve the journalistic tone."

---

## Step 4: Identify Key Vocabulary

Select 80-120 key words across the article for the click-to-highlight feature.

### What TO Tag

1. **High-frequency news vocabulary**
   - حکومت (government), انتخابات (elections), ملک (country)
   - فوجی (military), سیاسی (political), رہنما (leaders)
   - These words appear constantly in news articles

2. **Words that repeat within the article**
   - If a word appears 3+ times, tag every instance
   - Seeing it in different contexts reinforces learning
   - Example: میانمار (Myanmar) appearing throughout

3. **Numbers and time expressions**
   - پانچ (five), سات (seven), ہزار (thousand)
   - اتوار (Sunday), سال (year), ماہ (month)
   - Essential for understanding dates, statistics, timelines

4. **Country and organization names**
   - These are usually cognates but spelled in Urdu script
   - Helps learner recognize familiar concepts in new script
   - Example: امریکہ (America), اقوام متحدہ (United Nations)

5. **Useful abstract nouns**
   - صورتحال (situation), مقصد (purpose), نتیجہ (result)
   - Common across many topics, high return on learning

### What NOT to Tag

1. **Grammar particles and connectors**
   - کہ (that), کا/کی/کے (of), میں (in), سے (from)
   - Too common, would clutter the display
   - Better learned through grammar study

2. **Proper names of individuals**
   - Politician names, journalist names
   - Not transferable vocabulary

3. **Every single word**
   - Tagging everything defeats the purpose
   - 80-120 words is the sweet spot for one article

4. **Highly specialized terminology**
   - Words that only appear once in unusual contexts
   - Focus on vocabulary with reuse potential

### Create a Mapping

```
سیاسی → political
رہنما → leaders
فوجی → military
حکومت → government
انتخابات → elections
صورتحال → situation
```

---

## Step 5: Create HTML Structure

Use this template for each paragraph pair:

```html
<div class="paragraph-pair">
    <div class="urdu">
        [Urdu text with <span class="word" data-word="key">word</span> tags]
    </div>
    <div class="english">
        [English text with <span class="word" data-word="key">word</span> tags]
    </div>
</div>
```

**Critical**: The `data-word` attribute must match between Urdu and English spans for the same vocabulary item.

---

## Step 6: Add JavaScript for Highlighting

```javascript
// Click-to-highlight vocabulary linking
document.querySelectorAll('.word').forEach(word => {
    word.addEventListener('click', function() {
        const wordKey = this.dataset.word;

        // Remove all existing highlights
        document.querySelectorAll('.word.highlighted').forEach(el => {
            el.classList.remove('highlighted');
        });

        // Highlight all words with the same data-word value
        document.querySelectorAll(`.word[data-word="${wordKey}"]`).forEach(el => {
            el.classList.add('highlighted');
        });
    });
});

// Click elsewhere to clear highlights
document.addEventListener('click', function(e) {
    if (!e.target.classList.contains('word')) {
        document.querySelectorAll('.word.highlighted').forEach(el => {
            el.classList.remove('highlighted');
        });
    }
});
```

---

## Step 7: Styling Requirements

Essential CSS for Urdu text:

```css
.urdu {
    font-family: 'Noto Nastaliq Urdu', 'Jameel Noori Nastaleeq', serif;
    font-size: 1.4rem;
    line-height: 2.2;  /* Nastaliq needs extra line height */
    direction: rtl;
    text-align: right;
}
```

Load the font from Google Fonts:
```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Nastaliq+Urdu&display=swap" rel="stylesheet">
```

Highlight styling:
```css
.word {
    cursor: pointer;
    border-radius: 3px;
    padding: 0 2px;
    transition: all 0.2s ease;
}

.word:hover {
    background: rgba(212, 163, 115, 0.2);
}

.word.highlighted {
    background: #d4a373;
    color: #1a1a2e;
    font-weight: 600;
}
```

---

## Step 8: Serve Locally

```bash
# Python (recommended - reliable port binding)
python -m http.server 3004 --directory "C:\path\to\Urdu-Reader"

# Or Node.js
npx serve -l 3004 /path/to/Urdu-Reader
```

---

## Full Workflow Summary

```
BBC Urdu Article
       ↓
[Browser: get_page_text]
       ↓
Raw Urdu Text
       ↓
[Claude: Translate paragraph-by-paragraph]
       ↓
Urdu + English Pairs
       ↓
[Claude: Identify key vocabulary, create data-word mappings]
       ↓
Tagged HTML with vocabulary spans
       ↓
[Add CSS + JavaScript]
       ↓
Complete index.html
       ↓
[Serve on localhost]
       ↓
Interactive Reading Practice
```

---

## Time Estimate

- Article extraction: 2 minutes
- Translation: 5-10 minutes (depending on length)
- Vocabulary tagging: 10-15 minutes
- HTML assembly: 5 minutes
- **Total**: ~25-30 minutes per article

---

## Tips for Quality

1. **Don't over-tag**: 80-120 words is plenty. Tagging every word makes it cluttered.
2. **Repeat vocabulary**: If a word appears multiple times, tag all instances - seeing it in different contexts helps learning.
3. **Multi-word phrases**: Use hyphens in data-word for phrases: `data-word="civil-war"` for خانہ جنگی
4. **Test the highlighting**: Click through several words to verify Urdu-English pairs are correctly linked.

---

## Content Preservation Guidelines

**CRITICAL**: When modifying the app structure or adding new articles, follow these guidelines to prevent accidental content loss.

### Why Truncation Happens

When restructuring code (e.g., converting static HTML to JavaScript data objects, adding new features), manually rewriting content risks:
- Copying only partial content
- Missing paragraphs during conversion
- Fatigue-induced omissions in long articles

### Prevention Checklist

**Before making structural changes:**

1. **Count paragraphs** - Note the exact paragraph count of existing articles
   - Example: "Myanmar article has 22 paragraphs"

2. **Save a backup** - Copy the full original content somewhere safe before refactoring

3. **Make incremental changes** - Don't do everything at once:
   - ❌ Bad: Convert to JS + add selector + add new article in one step
   - ✅ Good: Convert existing content first → verify → then add new features

**After making structural changes:**

4. **Verify paragraph counts match** - Count paragraphs in the new structure

5. **Spot-check content** - Verify key details are present:
   - First and last paragraphs
   - Unique details from the middle (names, statistics, quotes)

6. **Test the app** - Load and scroll through to confirm all content renders

### Adding New Articles

When adding a new article to an existing multi-article setup:

1. **Don't touch existing article content** - Only add the new article object
2. **Copy-paste full extraction** - Don't manually retype or summarize
3. **Verify existing articles still complete** - Quick scroll-through check
