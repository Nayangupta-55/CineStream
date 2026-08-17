# Cine-Stream - Media Explorer

A Netflix-lite media discovery Single Page Application built with vanilla HTML, CSS, and JavaScript, consuming the [TMDB](https://developer.themoviedb.org/) REST API. Built as a sprint project focused on on-demand data hydration (infinite scroll) and API request throttling (debouncing).

No build step, no framework, no bundler — a single `cine-stream.html` file you can open in any browser.

## Live Demo
https://cine-stream-five.vercel.app/

## Features

### Phase 1 - Base Architecture & API Consumption
- Fetches the **Popular Movies** endpoint and renders results in a responsive CSS grid (poster, title, release year, rating).
- Search bar hitting the TMDB **Search** endpoint.

### Phase 2 - Performance & Persistence
- **Infinite scroll** via `IntersectionObserver` - automatically fetches and appends page N+1 as you scroll, instead of paginated buttons.
- **Debounced search** - waits 500ms after you stop typing before firing a request, so it doesn't spam the API on every keystroke.
- **Favorites** - heart any movie card to save it; state is persisted and synced to a dedicated `#/favorites` view.

### Phase 3 - AI & Asset Optimization
- **Lazy-loaded images** — every poster uses native `loading="lazy"` so offscreen images don't download until needed.
- **AI "Mood Matcher"** — describe a mood or vibe (e.g. *"feeling sad but want something action-packed"*), an LLM picks a single movie title, and the app silently searches TMDB for it and renders the result.

## Project Structure

cine-stream/
├── cine-stream.html   # entire app: markup, styles, and logic in one file
└── README.md

##  How It Works

| Concern | Implementation |
|---|---|
| Rendering | Vanilla JS, string-templated HTML re-rendered into a root `<div id="app">` on state change |
| Infinite scroll | `IntersectionObserver` watching a sentinel element at the bottom of the grid |
| Debouncing | Custom `debounce(fn, delay)` utility wrapping the search handler at 500ms |
| Routing | `location.hash` + `hashchange` listener toggles between Browse and Favorites views |
| Persistence | Key-value storage API (favorites + saved API key survive reloads) |
| Mood Matcher | Prompts an LLM to return `{"title": "..."}` as strict JSON, then feeds that title into the same TMDB search function used by the search bar |
| Lazy loading | Native `loading="lazy"` on every poster `<img>` |

##  Known Limitations / TODO

- **No OMDb adapter yet.** The fetch layer assumes TMDB's response shape. Adding OMDb support means writing a small adapter to normalize `Title/Year/imdbRating/Poster` into the shape the UI expects.
- **No movie detail view.** Cards show poster, title, year, and rating only - no synopsis/cast modal yet.
- **No sort/filter controls** (by genre, year, rating, etc.).
- **Single-user favorites.** Favorites are stored per-browser/session, not synced across devices or accounts.
- **Mood Matcher depends on LLM output quality.** If the model returns a title TMDB has no record of, the UI surfaces an error rather than silently failing.
- 
## Author

Nayan Gupta
