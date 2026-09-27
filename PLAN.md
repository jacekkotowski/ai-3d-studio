# Studio plan (status 2026-09-27)

## Done
- Starter kit: CLAUDE.md, skills idea-to-spec + build-preview, recipe presi-flight, library/rigs/flight.py (Blender 5, Eevee)
- Split of jobs: ai-film-lab makes the film (voice, cuts, captions, music); this studio only turns it into 3D.
  ai-film-lab `film final` writes out/final.timeline.json (frame-exact shot times, roles, titles).
- library/rigs/film_to_stops.py: timeline -> stops.json (film, hub = title/intro/closing, one stop per picture, visits).
- flight.py film mode: every stop is a screen playing its part of the film; the camera dives in, holds it full
  screen while it plays, glides on at the cut, closing back on the hub and played to the last frame, then the
  pull-back to the map. Round corners (squared as a screen fills the frame), colour halo, dot grid, links edge
  to edge. Only glide frames are rendered; ffmpeg lays the film's own frames and sound over the rest.
- Films done: 2026-09_trade-behind-war (draft), 2026-09_ai-on-my-terms (out/flight_film.mp4, 130.7 s).
- Public on GitHub: jacekkotowski/ai-3d-studio and jacekkotowski/ai-film-lab (branch main).
- Blender 5.2.1 at C:\Program Files\Blender (not on PATH). Draft ~2 min, full video ~10 min.

## Tomorrow: the map lights up as the story goes
Now every node is lit from the first frame, so the ending pull-back shows the same map as the opening.
Instead: nodes not yet visited are dim ghosts; each one lights up as the camera flies to it, the link
drawing itself out towards it; visited nodes stay lit; the final pull-back reveals the whole map lit.

1. **Dim level, one constant.** `DIM = 0.25`: what an unvisited node's emission is multiplied by (screen,
   frame, halo, title, link). The hub is lit from the start.
2. **Keep what gets animated.** `add_film_node` returns the node's emission sockets (screen, frame, halo
   strength, title); `add_link` returns its curve and emission socket. Each node already has its own
   materials, so no sharing to untangle.
3. **`light_up(nodes, links, visits, spans, fps)`**, beside `swap_screens`/`square_corners`. For each node's
   *first* visit only:
   - the glide into it starts at the previous visit's `leave` and ends at its `arrive`;
   - the link's `bevel_factor_end` goes 0 -> 1 over the first half of that glide, so it draws out from the
     node we are leaving;
   - the node's strengths go DIM -> 1 over the second half, finishing **at or before `arrive`**.
   Keyframe the socket `default_value`s and the curve's `bevel_factor_end`.
4. **Hard rule: full brightness by `arrive`.** From `arrive` on, the frame is replaced by the film's own; a
   node still brightening there would jump at the hand-over. Check it on the frame before `arrive`.
5. **Glides a bit slower:** `GLIDE_S` 1.5 -> 2.0. One constant. The glide is half before the cut and half
   after, so the full-screen stretches get 0.25 s shorter at each end, and no sound changes.
6. **Check:** `--stills` on ai-on-my-terms. `opening` should show the hub lit and the rest as ghosts; a
   `v0N_glide` a half-drawn link and a brightening node; `overview` everything lit. Then `--draft` (~2 min),
   and watch the first hand-over frame by frame as before.

Small, not cosmetic, while in there: **Shorts guard.** If the film is 180 s or less but film + 3 s pull-back
would pass 180 s, shorten the pull-back to fit (not below 1 s) and say so. Otherwise a Short silently
becomes an ordinary video.

## Later / optional
- Captions during the glides (the full-screen parts already carry the film's own)
- FLY.bat: drop final.timeline.json on it; finds Blender itself; convert -> stills -> draft
- 2.5D parallax for photo films: belongs in ai-film-lab's render (a depth map per still, cached in
  analysis/, model via models.py; Depth Anything V2 + onnxruntime), not here
- Recipes: plot3d, machine, photo-planes
