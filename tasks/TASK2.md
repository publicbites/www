# BookByte MVP2: Shareable Highlights & Text-to-Speech

## Overview
This milestone extends MVP1 with:
- Shareable highlighted text  
- Instant text-to-speech playback  
- Mobile-optimized UX and layout  

Future milestones will expand with AI-generated summaries, translations, advanced audio controls, and social features.

---

## Core New Features
- Highlight text within paragraphs  
- Generate shareable URLs preserving highlight ranges  
- Instant TTS playback  
- Mobile-first responsive layout with vertical action buttons  
- Standalone share pages for public viewing of individual paragraphs  

---

## Text Highlighting & Sharing

### User Flow
1. User highlights text via native browser selection  
2. Share button visually activates (white → orange)  
3. Clicking share copies URL to clipboard  
4. Toast notification appears: **“Link copied!”** for 3 seconds  

### Implementation Details
- Uses **react-highlight-selection**  
- Highlighted text remains selectable  
- URL params parsed to restore highlight ranges  

---

## Standalone Share Pages

### Routes
- **Feed:** `/`  
- **Standalone paragraph:** `/p/:paragraphId`  
- **With highlight:** `/p/:paragraphId?hl=start-end`  

### Features
- Single paragraph card view  
- Action buttons fully functional  
- Scrolling downward loads next paragraph from the same book  
- Infinite scroll enables sequential reading  

---

## Text-to-Speech (TTS) System

### OpenAI Configuration
- **Model:** `tts-1`  
- **Voice:** alloy  
- **Output Format:** MP3  
- **Storage:** S3  

### Generation Strategy

#### Phase 1 — Initial Load
- API returns only paragraphs with existing TTS  
- Guarantees instant playback  

#### Phase 2 — Background Generation
- New paragraphs without TTS enter Celery queue  
- Django Celery tasks generate audio asynchronously  
- Paragraphs become available after processing  

#### Phase 3 — Preloading
- Preload the next 2–3 audio files when the paragraph loads  
- Use `<audio preload="auto">`  
- Ensures instant playback as user scrolls  

### Audio Player
- Play/Pause button  
- Pause icon appears while active  
- Preloads audio for upcoming paragraphs  

---

## Backend Requirements (Django)

### Celery Tasks
- `generate_content_async(paragraph_id, generation_type)`  
- Trigger TTS generation when new paragraphs are viewed  

---

## MVP2 Scope (This Task Only)

### Included
- Text highlighting + shareable URLs  
- Standalone paragraph pages  
- OpenAI TTS (tts-1) integration  
- S3 storage with audio preloading  
- Unified content-generation pipeline  
- Sequential reading via standalone pages  
- Background TTS generation with Celery  

### Not Included (Future Milestones)
- AI-generated summaries  
- EN ↔ ZH translations  
- Collapsible book descriptions  
- Links to full book text files  
- User accounts  
- Social features  
- Advanced audio controls (speed, skip, etc.)  
- Offline/PWA mode  
- Multiple TTS voices  
- Image generation  
- Analytics  
- Search  
- Admin dashboard  
