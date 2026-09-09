# 📓 Dev log — js13k 2026 Froopyland

> All development steps, newest at the bottom.

## 2026-09-05

- [x] Concept agreed: Beth + Rick return to Froopyland (S3E9 *"The ABC's of Beth"*)
- [x] Competition theme checked: **Unicorns and Rainbows**
- [x] Public GitHub repo created: https://github.com/Ded0kl/js13k-2026-froopyland
- [x] GitHub Pages enabled: https://ded0kl.github.io/js13k-2026-froopyland/
- [x] Characters in one linear canvas style: Rick (star hair, monobrow), Beth, cute unicorns; the speaking character's mouth animates
- [x] Level skeleton: 5 floors, 3 rainbow ladders, portal on top
- [x] Dialog panel with speaker portrait and a line queue
- [x] Controls: tap-to-walk routes through ladders, joystick, WASD/arrows
- [x] Sound: WebAudio (steps, chime, neigh, hit, burp)

## 2026-09-06

- [x] **Start screen**: dark bg, FROOPYLAND title, green ▶ PLAY button, all four characters lined up below with name labels
- [x] Game no longer auto-starts; audio context initializes on the PLAY tap
- [x] **Story rewrite per canon**: 4 phases first — garage cables, cells, knife fight, DNA lights-out
- [x] Dialog written in the show's style; every level ends with an instruction line that stays on screen
- [x] Light start-screen background so the characters are visible; temporary debug level buttons added to the menu for testing
- [x] Live-site cache debugged (GitHub Pages served stale files; hard refresh needed)

## 2026-09-07

- [x] **Second act**: after the garage they enter Froopyland; Rick says it's child-proofed and nothing can go wrong — a giant winged pony grabs him into the nest
- [x] Rick-and-Morty-style dialog rewritten for every phase; Rick's "SEVEN trials — one per rainbow color" line added (theme flex)
- [x] **NEST**: one-bite full restart rule; round two flies faster; 20 kills to win; Rick drawn inside the nest after restart (was off-screen — fixed)
- [x] **CELLS**: five memory cells, carried **one at a time** to Rick; no auto-walk — the player walks and climbs everything; touching a froupie restarts the level
- [x] **GATE**: Simon-gate fixed (round progression bug), set to **5 rounds, one mistake = all reset**, fullscreen light board, honest 1 s pause between rounds
- [x] **Level order** reshuffled so active/passive alternate: GAR → NEST → GATE → CELLS → TRACE → TOMMY → DNA
- [x] Dialog panel resized for phones (font auto-shrink + honest fit checks; verified by line-wrap simulation on 4 screen sizes)
- [x] **TRACE** reworked: fullscreen light board, 36-dot zigzag, only dots + dialogs
- [x] **TOMMY**: pink arrow mechanic — take arrow → tap Beth to aim → tap Tommy to fire; 3 hits stop him, then walk up to finish; arrow respawns at the pedestal; keyboard input removed entirely (mouse/touch only); "RICK AND BETH" handwritten-style subtitle on the menu
- [x] **DNA** reworked into a **6×12 portrait cable puzzle** (top-left → bottom-right), tuned for phone width; tap coordinates unified with the drawn grid
- [x] All phase-0..6 smoke tests run in Node with stubbed canvas/AudioContext
- [x] **Text deduplication**: shared phrase variables (`*burp* `, `Wubba lubba dub dub!`, `flip it AND its neighbour`, `chalk door`, `say sorry`, `Walk it off…`, `hatchlings`, `zap them`) reused across all dialog arrays

## 2026-09-08

- [x] **Size reduction (see DESIGN.md for details):**
  - measured: raw zip 14.6 KB → needs work to reach 13,312 B
  - tools: Node strip script + `terser -c passes=4,unsafe,toplevel -m toplevel` + `zip -9 -X`
  - debug buttons stripped only in the submission build (kept in repo for testing)
  - toplevel mangling alone saved ~660 B of zip
  - minified build smoke-tested (boot → PLAY → dialogs → click produces sound)
  - **final zip: 13,277 / 13,312 bytes (35 B spare)** — `js13k-froopyland.zip`
- [x] Category research for the submit form (Desktop+Mobile+Audio already satisfied; Wavedash = publish until Sep 20; Online = optional multiplayer via their WS relay; WebXR = separate VR game)
- [ ] Submit on js13kgames.com/2026 (deadline Sep 13, 13:00 CEST)
- [x] **Trailer**: cut 13–26s + two stray menu flashes (40–42.1s, 48.6–51.2s) → 45.3s; soundtrack = the game's own simplified theme rendered via `_theme_game_render.js` (exact `musN` square-wave replica, BPM 82.4, 4 loops) → `froopyland-trailer-final.mp4`
- [x] **BUGFIX (critical)**: in full playthroughs the DNA level (ph=5) could throw the player back to CELLS — the froupie chase/catch code ran during DNA because ph5 fell through the `update()` phase guards (only hit in real runs; debug DNA jump reset froupies differently). Fix: one-line guard `if(ph==5)return;` (+15 B minified; zip now **13,282 / 13,312 B**). Regression sims: `sim_dna.js`, `sim_cells.js` (pickup + froupie-catch still work)
- [ ] Optional: publish on Wavedash before Sep 20 ($10 credit for every valid entry)
- [x] **Submission build rebuilt with the melody**: zip's game.js was stale (pre-melody). Stripped debug buttons from dev source, minified via terser (34,431 B raw — melody costs ~785 B), and **packed with 7z -mx=9 -mfb=258 -mpass=15 instead of Info-ZIP: 13,285 / 13,312 B (27 B spare)**. Smoke-tested from the unzipped artifact: boot, PLAY, theme notes firing, DNA guard present.
