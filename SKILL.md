# Letter Tracker - Reading App for Toddlers

## Overview

A single-page web app for tracking a young child's (age 2-4) letter learning progress, built following Erik Hoel's phonics-based reading instruction approach. The app tracks letter recognition status, provides spaced repetition reminders, and includes a "Word Practice" feature based on Hoel's "complete-the-sentence game."

**Live URL**: Deployed via GitHub Pages
**Repository**: `fdhcsocnqwdn3/letter-tracker`

## Core Methodology

Based on Erik Hoel's "How to Birth a Reader" series:
- **Phonics-first**: Focus on letter sounds (phonics) before letter names
- **Uppercase before lowercase**: Easier visual distinction for young learners
- **Frequency-ordered**: Letters taught in order of English language frequency (E, T, A, O, I, N, S, H, R...)
- **Spaced repetition**: Review intervals based on mastery level (red=daily, amber=2 days, green=5 days)
- **Complete-the-sentence game** (Tier 2): Engaging sentences where child reads the final word using only known letters

## Features

### 1. Letter Tracking (Uppercase & Lowercase tabs)
- 26 letter cards with Red/Amber/Green status
- Tracks both phonics (sound) and name recognition separately
- Visual progress stats (green/amber/red counts)
- Search/filter functionality
- Click to open detail modal and update status

### 2. Spaced Repetition
- Calculates "due for review" based on last review date and status
- Visual badges show when letters need practice
- Review intervals: Red (1 day), Amber (2 days), Green (5 days)

### 3. Progress Visualisation
- Progress bar showing percentage of green letters
- 30-day trend chart showing learning progress over time
- Stats cards with counts per status

### 4. Word Practice Tab
- **Complete-the-sentence game**: Shows engaging sentence prompts where child reads the final word
- Only shows words that can be decoded using child's known (green) letters
- ~120 CVC words organised by word family (-at, -an, -en, -ig, -og, -ug, etc.)
- 3-4 story-like sentences per word following Hoel's style
- Navigation: Next/Prev word, Next/Prev sentence
- Shows word family info (e.g., "Word family: -at")
- Empty state with guidance when not enough letters are known

### 5. Data Management
- Export data to JSON file
- Import data from JSON file
- All data persisted in localStorage

## Technical Details

### Architecture
- **Single HTML file** containing all CSS and JavaScript
- No build process or dependencies
- Deployed directly to GitHub Pages via `.github/workflows/deploy.yml`
- Mobile-responsive design

### Data Structure
```javascript
// Letter data (stored in localStorage)
letterData = {
  "E": {
    "uppercase-phonics": "green",      // red | amber | green
    "uppercase-name": "amber",
    "lowercase-phonics": "red",
    "lowercase-name": "red",
    "uppercase-phonics-last-reviewed": "2025-01-20T10:00:00Z",
    "lowercase-phonics-last-reviewed": null,
    "history": [...]
  },
  // ... 26 letters
}

// Word database
WORD_DATABASE = {
  words: [
    { word: "cat", letters: ["c","a","t"], type: "cvc", family: "-at" },
    // ~120 words
  ],
  sentences: {
    "cat": [
      "I heard a noise in the garden and when I looked, it was a fluffy...",
      "The dog was chasing something through the house - it was chasing a...",
      // 3-4 sentences per word
    ],
    // ...
  }
}
```

### Key Functions
- `getGreenLetters()` - Returns set of lowercase letters with green uppercase phonics status
- `canDecodeWord(word, greenLetters)` - Checks if word can be read with available letters
- `getAvailableWords()` - Filters word database to decodable words
- `renderPracticeWord()` - Displays current word with sentence prompt
- `isReviewDue(status, lastReviewDate)` - Calculates if letter needs practice

### Word Families Included
| Vowel | Families |
|-------|----------|
| A | -at, -an, -ad, -ap, -ag |
| E | -en, -et, -ed |
| I | -ig, -it, -in, -ip |
| O | -og, -ot, -op, -ob |
| U | -ug, -un, -ut, -ub, -um |

Plus 2-letter words: at, it, in, on, up, an, am, if, is, us

## File Structure

```
Raph Reading/
├── index.html          # Complete app (HTML + CSS + JS)
├── SKILL.md            # This file
└── .github/
    └── workflows/
        └── deploy.yml  # GitHub Pages deployment
```

## Usage

1. Open the app in a browser
2. Start with **Uppercase** tab - mark letters green as child learns their sounds
3. Use **Word Practice** tab once child knows a few letters (ideally at least one vowel + consonants)
4. Return to letter tracking to mark review dates and update statuses
5. Check **Lowercase** tab as child progresses

## Extending

### Adding new words
Add to `WORD_DATABASE.words` array:
```javascript
{ word: "newword", letters: ["n","e","w","w","o","r","d"], type: "cvc", family: "-ord" }
```

And add corresponding sentences:
```javascript
"newword": [
    "An engaging sentence that ends with...",
    "Another story-like prompt that naturally completes with...",
    "A third vivid sentence ending in...",
]
```

### Sentence style guidelines (per Hoel)
- Set a vivid scene that makes the child want to know the word
- Make sentences relevant and engaging for young children
- Include familiar contexts: animals, family, play, food, bedtime
- Sentence should flow naturally into the target word

## References

- [Erik Hoel - How to Teach Your Kid to Read (Part 1-3)](https://www.theintrinsicperspective.com/)
- CVC word lists from phonics education resources
- Dolch sight words list
