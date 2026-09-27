# Studio starter kit

Ideas → 3D presentations, locally, with Blender + Claude Code.

## Setup (once)
Three programs, no Python packages. Blender has its own Python, and the other scripts use only the standard library.
```
winget install BlenderFoundation.Blender
winget install Gyan.FFmpeg
winget install Python.Python.3.12
```
1. Make sure `blender` runs from a terminal. The winget install does not add it to PATH, so add `C:\Program Files\Blender` to PATH, or type the full path to `blender.exe`.
2. Put this folder wherever you keep projects and open Claude Code in it. It reads `CLAUDE.md` automatically.
3. Optional: copy `skills/*` into `.claude/skills/` so they show up as `/idea-to-spec` and `/build-preview`.

## From an ai-film-lab film
[ai-film-lab](https://github.com/jacekkotowski/ai-film-lab) makes the film: your photos, clips and voice, cut, captioned and set to music. Its `film final` writes `out/final.timeline.json` beside the video. Then:
```
python library/rigs/film_to_stops.py "<film>/out/final.timeline.json" projects/<yyyy-mm_slug>
blender -b -P library/rigs/flight.py -- projects/<yyyy-mm_slug>/stops.json --stills
```
The film becomes a map. The hub plays the title and your intro, and later your closing. Each picture's part plays on its own screen around the hub. The camera dives into each screen as its part starts, holds it full-screen while it plays, and flies to the next one at the cut. `--draft`/`--video` then write `flight_film.mp4`, which uses the film's own frames wherever a screen is full-screen and the film's own sound throughout. Titles come from the picture file names; edit them in `stops.json`.

## First run
```
blender -b -P library/rigs/flight.py -- projects/2026-09_ai-in-obsidian/stops.json --stills
```
Stills land in `projects/2026-09_ai-in-obsidian/preview/`.

## New idea
Make `projects/<yyyy-mm_slug>/input/idea.md`, then tell Claude: *"run idea-to-spec on <slug>"*.

## Plan
See `PLAN.md` for what is done and what comes next.

## Recipes
- presi-flight ✅
- plot3d, machine, photo-planes: planned, not built yet
