# Maze

## What it is

A puzzle of pathways with a START and a FINISH. The kid traces a route through, choosing turns and avoiding dead ends.

## Age range

**3-12 years**. Massively scalable.

| Age | Grid | Path | Notes |
|---|---|---|---|
| 3-4 yr | 6×6 | Single solution, very wide path, 4-6 turns | "Help the bunny find its carrot" |
| 4-5 yr | 8×8 | Single solution, 8-10 turns, narrower path | Themed |
| 5-6 yr | 10×10 | Multi-path (2-3 dead ends), 10-15 turns | Story-integrated |
| 6-8 yr | 12×12 | Multi-path with checkpoints | Pick up items along the way |
| 8-12 yr | 15×15+ | Complex, multi-page, mini-puzzles at junctions | Escape-room-style |

## Skills it builds

- Visual scanning
- Planning ahead / problem solving
- Fine motor
- Persistence

## Format spec

Generate as **inline SVG**. Use a grid-based maze algorithm (depth-first, Prim's, recursive backtracker — any). Required:

- Clear START arrow + label (top-left or top)
- Clear FINISH flag + label (bottom-right or bottom)
- Walls: solid lines, ~3px stroke
- Path width: 2x wall stroke at minimum (kids need room to draw)
- Optional: theme-art around the border (waves, stars, leaves) that doesn't interfere with the maze

## Quality bar

- **One clear correct path** (or a stated number of correct paths if multi-solution)
- **Real challenge** — dead ends should feel plausible, not obvious
- **Theme-integrated** — "help the dragon find its hoard", "guide the spaceship to its planet"
- **Solvable without trial-and-error frustration** — wide enough that pencil mistakes don't ruin the page
- **Optional: solution key on a separate page** for the parent

## Variations

- **Pick-up-and-deliver**: kid must visit certain dots in order, then exit
- **Multi-page maze**: maze continues across pages (advanced)
- **Maze + cipher**: the path spells out a word (letters at junctions)
- **Maze + math**: each fork has a math problem; right answer = correct path
- **Reverse maze**: kid draws the maze; parent solves
- **Story maze**: story unfolds along the path with mini-prompts at checkpoints

## Material requirements

Printer + pencil. That's it.

## Print considerations

- High contrast: solid walls, white path
- Color mode: lines stay black; color reserved for theme art
- B&W friendly by default
- Solution: include on a follow-up page or omit entirely (most kids prefer no spoilers)

## Generation approach

1. Pick grid size based on age
2. Generate maze using a standard algorithm (recursive backtracker is reliable)
3. Verify there's a unique path from START to FINISH (or document the # of paths)
4. Place theme decorations *outside* the grid
5. Render as SVG with proper aspect ratio for the print size

## Example brief

> Kid: Maya, 4yr
> Interests: ocean
> Theme: ocean week

→ Title: "Help Maya's Megalodon Find Lunch!"
→ 8×8 grid maze, single path with 2 dead ends
→ Megalodon at START (top-left), school of fish at FINISH (bottom-right)
→ Wave decorations around border, a few seaweed silhouettes outside the maze
→ Path width: 0.6", wall thickness: 3px
→ Footer: "When you find the fish, color in the megalodon!"
