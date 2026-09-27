# Studio plan (status 2026-09-27)

## Done
- Starter kit: CLAUDE.md, skills idea-to-spec + build-preview, recipe presi-flight, library/rigs/flight.py (Blender 5, Eevee)
- First project: projects/2026-09_ai-in-obsidian (hub + Claudian, Git, Connectors, Skills, Local vectors); preview stills rendered
- flight.py modes: --stills, --draft, --video, --frames=, --blend
- Blender 5.2.1 works on this PC (C:\Program Files\Blender, not on PATH). 8 stills in 20 s.
- Split of jobs: ai-film-lab makes the film (voice, cuts, captions, music); this studio only turns it into 3D.
- ai-film-lab `film final` writes out/final.timeline.json (frame-exact shot times, roles, titles).
- library/rigs/film_to_stops.py: timeline -> stops.json (film, hub = title/intro/closing, one stop per picture, visits).
- flight.py film mode: every stop is a screen playing its part of the film; the camera dives in, holds full screen while it plays, glides out at the cut, closing back on the hub; ffmpeg lays the original film over the full-screen stretches and adds its sound -> <name>_film.mp4.
- projects/2026-09_trade-behind-war: film-mode stills approved in principle; draft rendering.

## Next (simplest first)
1. Draft approved 2026-09-27. Next film: `film final` -> film_to_stops.py -> --stills -> --draft.
2. Done 2026-09-27: round corners + colour halo + dot grid, links edge to edge, only glide frames rendered, the outro plays to the last frame and the pull-back to the map comes after it (sound padded, never cut).
3. projects/2026-09_ai-on-my-terms: draft, then `--video` -> out/flight_film.mp4.

## Later / optional
- Captions during the glides (the full-screen parts already carry the film's own)
- Face bubble in a corner
- Recipes: plot3d, machine, photo-planes
