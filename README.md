# The Unfound

*Anything you feel can become something you create.*

An interactive sketchbook website: scroll from an editorial intro onto a blank page, draw/scribble/write, name what you made, and revisit it in your own gallery. No accounts — everything is saved in the browser (IndexedDB).

## Run it

```bash
npm install
npm run dev            # local dev server
npm run build          # production build → dist/
npm run build:single   # everything inlined into one file → dist-single/index.html
```

## How it's put together

```
src/
  App.jsx                    page flow, saving, editing, deleting, toasts
  lib/brushes.js             drawing engine (pencil, marker, crayon, spray, eraser, text)
  lib/storage.js             IndexedDB wrapper with in-memory fallback
  lib/useReveal.js           scroll-reveal for text + self-drawing doodles
  components/
    Nav, Hero, Marquee        section 01 — the introduction
    PaperIntro                scroll-driven "the page grows until it's the screen" transition
    LetItOut                  section 02 — the canvas section (shortcuts, hints, status)
    canvas/DrawingCanvas      the canvas itself (pointer events, undo/redo, text, resize)
    canvas/Toolbar, Icons     the small toolbar
    NameModal                 "What should we call this little thing?"
    Gallery, ArtViewer        section 03 — "Look what you let out." + open/edit/delete/download
    Manifesto, Footer         about + a gentle note that this isn't a replacement for support
    Doodles                   the hand-drawn SVG doodle library
  styles/                     base, hero, canvas, gallery, sections
```

### Drawing engine notes
- Artwork is stored as **vector actions** in a fixed logical space (1600×1050 landscape, or 1000×1400 portrait on phones). It is re-rendered at any screen size, so resizing never loses or blurs the drawing, and saved pieces can be reopened with full undo history intact.
- Textured brushes (pencil grain, crayon wax, spray) use a **seeded random generator**, so a replayed stroke looks exactly like it did live.
- Marker strokes are drawn opaque on a scratch layer and laid down translucent, so a stroke never darkens where it crosses itself.
- Works with mouse, trackpad, touch and pen (pen pressure changes line width). Uses coalesced pointer events for smooth lines.
- Clear is an undoable action. The current page autosaves, so a refresh doesn't lose work.

### Shortcuts
`P` pencil · `M` marker · `C` crayon · `S` spray · `E` eraser · `T` words · `[` `]` size · `Ctrl/⌘+Z` undo · `Ctrl/⌘+Shift+Z` redo

### Fonts
Instrument Serif, Instrument Sans and Caveat, self-hosted via Fontsource (no external requests).
