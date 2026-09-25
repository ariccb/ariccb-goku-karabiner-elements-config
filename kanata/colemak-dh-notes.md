# Colemak DH — Base Layer Notes

How the Kanata config maps physical QWERTY key positions to Colemak DH output.

---

## Layout at a glance

Physical keyboard (QWERTY labels on the keycaps):

```
 q   w   e   r   t       y   u   i   o   p
 a   s   d   f   g       h   j   k   l   ;
 z   x   c   v   b       n   m   ,   .   /
```

What Kanata produces (Colemak DH letters):

```
 q   w   f   p   b       j   l   u   y   ;
 a   r   s   t   g       m   n   e   i   o
 z   x   c   d   v       k   h   ,   .   /
```

---

## Key-by-key mapping

### Top row

| Physical key | QWERTY label | Colemak DH output | Changed? |
|---|---|---|---|
| `q` | Q | q | — |
| `w` | W | w | — |
| `e` | E | **f** | ✦ |
| `r` | R | **p** | ✦ |
| `t` | T | **b** | ✦ |
| `y` | Y | **j** | ✦ |
| `u` | U | **l** | ✦ |
| `i` | I | **u** | ✦ |
| `o` | O | **y** | ✦ |
| `p` | P | **;** | ✦ |

### Home row

| Physical key | QWERTY label | Colemak DH output | Changed? |
|---|---|---|---|
| `a` | A | a | — |
| `s` | S | **r** | ✦ |
| `d` | D | **s** | ✦ |
| `f` | F | **t** | ✦ |
| `g` | G | g | — |
| `h` | H | **m** | ✦ |
| `j` | J | **n** | ✦ |
| `k` | K | **e** | ✦ |
| `l` | L | **i** | ✦ |
| `;` | ; | **o** | ✦ |

### Bottom row

| Physical key | QWERTY label | Colemak DH output | Changed? |
|---|---|---|---|
| `z` | Z | z | — |
| `x` | X | x | — |
| `c` | C | c | — |
| `v` | V | **d** | ✦ |
| `b` | B | **v** | ✦ |
| `n` | N | **k** | ✦ |
| `m` | M | **h** | ✦ |
| `,` | , | , | — |
| `.` | . | . | — |
| `/` | / | / | — |

### Unchanged keys

Numbers (1–0), punctuation (`` ` ``, `-`, `=`, `[`, `]`, `\`, `'`), and all modifier / special keys pass through unmodified.

---

## ANSI vs ISO

This Kanata config uses **ANSI Colemak DH** — the standard variant for ANSI keyboards (US-style, no extra key between left Shift and Z).

The original Karabiner config used **Colemak DH ISO-extended**, which applies an "angle mod" to the bottom-left row:

```
ISO-extended bottom row (physical → output):
  `→z   z→x   x→c   c→d   v→v   b→`
```

The key difference is that the ISO variant shifts Z/X/C/D one position right and places Z on the grave/tilde key. This freed the physical `b` position for the backtick character.

### If you need the ISO variant

Edit the `defsrc` and `deflayer colemak-dh` to include the ISO extra key and shift the bottom row:

```lisp
;; defsrc bottom row (ISO):
  lsft 102d z    x    c    v    b    n    m    comm dot  slsh rsft
;;      ^^^^  extra ISO key (non_us_backslash / §)

;; deflayer colemak-dh bottom row (ISO):
  @lshf z   x    c    d    v    grv  k    h    comm @pdot slsh @rshf
;;      z→z x→x  c→c  d→d  v→v  `    ← backtick on the b position
```

---

## Why Kanata remaps instead of the OS

In the Karabiner setup, the OS Colemak DH input source handled letter remapping, and Karabiner-Elements operated on physical key codes *before* the input source translated them. Shortcut outputs (e.g., Cmd+Z for undo) were sent as physical key codes and the input source re-translated them.

Kanata takes a different approach: it intercepts physical key events, remaps them via `deflayer`, and the OS receives the final key codes directly. The OS keyboard layout should therefore be set to **US QWERTY** — otherwise letters would be remapped twice.

This means:
- **Macros** output literal QWERTY key codes (e.g., `z` produces the letter Z).
- **Shortcut chords** output the intended shortcut directly (e.g., `M-z` = Cmd+Z).
- The Colemak DH letter mapping lives entirely in Kanata's `deflayer colemak-dh`.

---

## Finger assignment reference

```
         Left hand                    Right hand
  Pinky Ring Mid  Index Index   Index Index Mid  Ring Pinky
   q     w    f    p  b           j    l    u    y    ;
   a     r    s    t  g           m    n    e    i    o
   z     x    c    d  v           k    h    ,    .    /
                    ▴ Thumb ▴
```

Colemak DH optimises for:
- High-frequency letters on the home row (e, t, a, o, i, n, s, r)
- Reduced lateral index-finger stretch compared to standard Colemak (D and H moved to comfortable curl positions)
- Same-finger bigrams minimised
