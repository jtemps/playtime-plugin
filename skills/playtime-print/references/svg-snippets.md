# SVG snippets

Copy-paste decorative motifs for inline use. All are stroke-only by default; fill with `var(--accent)` if you want color.

## Wave border (top or bottom)

```svg
<svg viewBox="0 0 600 30" width="100%" height="30">
  <path d="M0 15 Q 25 0, 50 15 T 100 15 T 150 15 T 200 15 T 250 15 T 300 15 T 350 15 T 400 15 T 450 15 T 500 15 T 550 15 T 600 15"
        fill="none" stroke="currentColor" stroke-width="2" />
</svg>
```

## Dotted divider

```svg
<svg viewBox="0 0 600 4" width="100%" height="4">
  <line x1="0" y1="2" x2="600" y2="2" stroke="currentColor" stroke-width="2" stroke-dasharray="4 6"/>
</svg>
```

## Sun

```svg
<svg viewBox="0 0 60 60" width="60" height="60">
  <circle cx="30" cy="30" r="12" fill="none" stroke="currentColor" stroke-width="2"/>
  <g stroke="currentColor" stroke-width="2" stroke-linecap="round">
    <line x1="30" y1="6" x2="30" y2="14"/>
    <line x1="30" y1="46" x2="30" y2="54"/>
    <line x1="6" y1="30" x2="14" y2="30"/>
    <line x1="46" y1="30" x2="54" y2="30"/>
    <line x1="13" y1="13" x2="19" y2="19"/>
    <line x1="41" y1="41" x2="47" y2="47"/>
    <line x1="13" y1="47" x2="19" y2="41"/>
    <line x1="41" y1="19" x2="47" y2="13"/>
  </g>
</svg>
```

## Cloud

```svg
<svg viewBox="0 0 100 50" width="100" height="50">
  <path d="M20 35 Q10 35 10 25 Q10 15 20 17 Q22 7 35 10 Q42 0 55 5 Q70 5 72 15 Q85 15 85 25 Q85 35 75 35 Z"
        fill="none" stroke="currentColor" stroke-width="2"/>
</svg>
```

## Star (5-point)

```svg
<svg viewBox="0 0 40 40" width="40" height="40">
  <polygon points="20,3 25,15 38,16 28,25 31,38 20,30 9,38 12,25 2,16 15,15"
           fill="none" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
</svg>
```

## Leaf

```svg
<svg viewBox="0 0 40 60" width="40" height="60">
  <path d="M20 5 Q5 25 20 55 Q35 25 20 5 Z" fill="none" stroke="currentColor" stroke-width="2"/>
  <line x1="20" y1="5" x2="20" y2="55" stroke="currentColor" stroke-width="1.5"/>
</svg>
```

## Heart

```svg
<svg viewBox="0 0 40 36" width="40" height="36">
  <path d="M20 33 L4 17 Q-2 8 8 4 Q15 2 20 11 Q25 2 32 4 Q42 8 36 17 Z"
        fill="none" stroke="currentColor" stroke-width="2"/>
</svg>
```

## Frame border

```svg
<svg viewBox="0 0 600 800" width="100%" height="100%" preserveAspectRatio="none"
     style="position:absolute; top:0; left:0; pointer-events:none;">
  <rect x="6" y="6" width="588" height="788" fill="none"
        stroke="currentColor" stroke-width="3" stroke-dasharray="2 6" rx="8"/>
</svg>
```

## Speech bubble

```svg
<svg viewBox="0 0 200 110" width="200" height="110">
  <path d="M10 20 Q10 5 25 5 H175 Q190 5 190 20 V70 Q190 85 175 85 H80 L60 105 L65 85 H25 Q10 85 10 70 Z"
        fill="white" stroke="currentColor" stroke-width="2.5"/>
</svg>
```

## Dot grid (for connect-the-dots)

Generated procedurally based on the path you compute. Each dot:

```svg
<g class="dot" transform="translate(150, 220)">
  <circle r="7" fill="currentColor"/>
  <text x="14" y="5" font-size="20" font-family="Georgia,serif" fill="currentColor">7</text>
</g>
```

## Maze cell wall (for use in generated mazes)

```svg
<line x1="X1" y1="Y1" x2="X2" y2="Y2" stroke="currentColor" stroke-width="3"/>
```

## Animal silhouettes (simple)

For megalodon (use as guide for connect-the-dots path or coloring outline):

```svg
<svg viewBox="0 0 200 100" width="200" height="100">
  <path d="M10 50 Q40 30 80 35 Q120 30 160 50 L180 30 L185 50 L180 70 L160 50 Q120 70 80 65 Q60 70 30 80 Z"
        fill="none" stroke="currentColor" stroke-width="2"/>
  <circle cx="35" cy="50" r="2" fill="currentColor"/>
  <path d="M10 50 L20 45 L30 50 L20 55 Z" fill="currentColor"/>  <!-- mouth -->
</svg>
```

(Adapt these for whatever animal/object the activity needs.)
