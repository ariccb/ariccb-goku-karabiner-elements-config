# Karabiner → Kanata Feature Mapping

A feature-by-feature comparison of the original Karabiner-Elements / Goku config and the Kanata port.

---

## Ported features

### Base layer — Colemak DH

| Karabiner | Kanata | Notes |
|---|---|---|
| macOS Colemak DH ISO-extended input source does letter remapping | `deflayer colemak-dh` remaps physical QWERTY → Colemak DH | Kanata uses ANSI Colemak DH; the ISO angle-mod (z on grave key) is not replicated — see `colemak-dh-notes.md` |

### Tap-hold dual-role keys

| Key | Tap | Hold | Karabiner | Kanata |
|---|---|---|---|---|
| Tab | Esc | Hyper (⌃⌥⌘⇧) | `[:##tab :!CTOleft_shift nil {:alone :escape}]` | `@htab (tap-hold-press … esc (multi lsft lctl lalt lmet))` |
| CapsLock | Tab | Ctrl+Alt | `[:##caps_lock :!Tleft_option nil {:alone :tab}]` | `@hcap (tap-hold-press … tab (multi lctl lalt))` |
| Left ⌘ | Enter | Left ⌘ | `[:##left_command :left_command nil {:alone :return_or_enter}]` | `@lcmd (tap-hold-press … ret lmet)` |
| Right ⌘ | Delete Fwd | Right ⌘ | `[:##right_command :right_command nil {:alone :delete_forward}]` | `@rcmd (tap-hold-press … del rmet)` |
| Left ⇧ | `[` | Left ⇧ | `[:##left_shift :left_shift nil {:alone :##open_bracket}]` | `@lshf (tap-hold-press … lbrc lsft)` |
| Right ⇧ | `]` | Right ⇧ | `[:##right_shift :right_shift nil {:alone :##close_bracket}]` | `@rshf (tap-hold-press … rbrc rsft)` |

### Symbols & numbers layer (Space held)

| Karabiner | Kanata |
|---|---|
| `:spacebar-mode` simlayer | `@sspc (tap-hold-press … spc (layer-while-held symbols))` |

Full layout preserved — see `deflayer symbols` in `kanata.kbd`. The grid maps physical positions to:

```
`  !  <  >  =  ^    *  7  8  9  :  -  +
S⇥ #  [  (  )  ]    &  4  5  6  -  +  \
@  $  {  }  %  ~  | 1  2  3     *
                        0  .
```

### Navigation layer (A held)

| Karabiner | Kanata |
|---|---|
| `:a-mode` simlayer | `@anav (tap-hold-press … a (layer-while-held navigation))` |

| Position (Colemak DH) | Output | Kanata key |
|---|---|---|
| n (phys j) | ← | `lft` |
| e (phys k) | ↓ | `down` |
| u (phys i) | ↑ | `up` |
| i (phys l) | → | `rght` |
| l (phys u) | Home | `home` |
| y (phys o) | End | `end` |
| h (phys m) | Page Up | `pgup` |
| . (phys dot) | Page Down | `pgdn` |
| , (phys comm) | Show App Windows | `M-A-down` (macOS only) |
| r (phys s) | Alt / F5 | `@ns` tap-hold |
| s (phys d) | Shift / F6 | `@nd` tap-hold |
| t (phys f) | Cmd / F7 | `@nf` tap-hold |
| g (phys g) | Ctrl / F8 | `@ng` tap-hold |
| x–v (phys z–v) | F1–F4 | direct |
| w–b (phys w–t) | F9–F12 | direct |
| o (phys ;) | ⌘Tab (app switch) | `@aptab` |
| ' (phys ') | ⌘⇧Tab (reverse) | `@aptbr` |
| v (phys b) | Prev tab (⌘⇧[) | `@ptab` |
| k (phys n) | Next tab (⌃Tab) | `@ntab` |
| m (phys h) | Prev tab (⌃⇧Tab) | `@nptab` |
| / | `\|` (pipe) | `S-bksl` |

### Simultaneous-key combos (chords)

| Keys (physical) | Output | Karabiner | Kanata |
|---|---|---|---|
| w+e | Undo | `[[:w :e] :!Cgrv]` | `(w e) @undo` |
| e+r | Redo | `[[:e :r] :!SCgrv]` | `(e r) @redo` |
| z+x | Cut | `[[:z :x] :!Cx]` | `(z x) @cut` |
| x+c | Copy | `[[:x :c] :!Cv]` | `(x c) @copy` |
| c+v | Paste | via `[[:x :c] :!Cv]` | `(c v) @paste` |
| z+b | Paste plain | `[[:z :v] :!COSv]` | `(z b) @pstpl` |
| z+c | Select All | `[[:grv :c] :!Ca]` | `(z c) @selal` |
| i+o | `-` | `[[:i :o] :hyphen]` | `(i o) min` |
| k+l | `=` | `[[:k :l] :equal_sign]` | `(k l) eql` |
| ,+. | `_` | `[[:comm :period] :!Shyphen]` | `(comm dot) S-min` |
| .+/ | `?` | `[[:period :slsh] :!Sslsh]` | `(dot slsh) S-slsh` |
| /+RShift | `!` | `[[:slsh :rsft] :!S1]` | `(slsh rsft) S-1` |
| m+, | `"` | `[[:m :comm] :!Squot]` | `(m comm) S-quot` |
| f+g | `@` | `[[:f :g] :!S2]` | `(f g) S-2` |
| 9+0 | Toggle terminal | `[[:9 :0] :!Tb]` | `(9 0) C-grv` |
| 8+9 | Prev terminal tab | `[[:8 :9] :!SCopen_bracket]` | `(8 9) M-S-lbrc` |
| 0+- | Next terminal tab | `[[:0 :hyphen] :!SCclose_bracket]` | `(0 min) M-S-rbrc` |
| 8+9+0 | New terminal | `[[:8 :9 :0] :!STb]` | `(8 9 0) C-S-grv` |

### Leader key macros (Period held)

| Key | Output | Status |
|---|---|---|
| .+a | `() => {` | ✅ `@arfn` |
| .+s | `function funcName() { return }` (selects name) | ✅ `@exfn` |
| .+b | Fenced code block (```) | ✅ `@cblk` |
| .+f | `- [ ]` Markdown checkbox | ✅ `@mbox` |
| .+c | `<div></div>` | ✅ `@htdv` |
| .+g | `<tag></tag>` (multi-cursor select) | ✅ `@httg` |
| .+, | `console.log()` | ✅ `@clog` |
| .+m | `/** */` block comment | ✅ `@bcmt` |

### Email address combos

| Keys (physical) | Email | Status |
|---|---|---|
| h+j | `aric@collegiumbuilt.com` | ✅ `@eml1` |
| m+n | `aric@helixirlabs.com` | ✅ `@eml2` |
| y+u | `aric@ariccb.dev` | ✅ `@eml3` |
| t+y | `aricbouwers@outlook.com` | ✅ `@eml4` |

---

## Deferred / not ported

These Karabiner features have no direct Kanata equivalent or require OS-specific workarounds.

| Feature | Karabiner | Why deferred | Workaround |
|---|---|---|---|
| **Double-press ⌘Q to quit** | Variable + delayed action + notification | Kanata has no notification API and no way to intercept modifier+key combos for tap-dance | Rely on macOS "Ask to keep changes" or use a separate tool |
| **Adaptive keys** (p→t, b→t, l→k, s→c, f→s, u→e/y, d→g, t→b, c→s sequences) | Per-key variable + delayed action mode | Complex stateful logic; Kanata `sequence` feature doesn't replicate the "override next key only" pattern | Could be partially emulated with `fork` + `switch` + virtual keys — substantial effort |
| **Application launching** (hold Q + key → open app) | `:apps` simlayer + `osascript` | Requires `danger-enable-cmd yes` and OS-specific shell commands | Add `(cmd open -a "App Name")` aliases per app |
| **Device-specific rules** (Apple vs Leeloo vs Kinesis etc.) | `:devices` conditions per vendor/product ID | Kanata identifies devices by path, not vendor ID; configuration differs | Use `defdevice` in Kanata to match specific keyboards |
| **Input-source conditions** (Colemak DH vs ABC vs Matrix) | `:input-sources` + `:condi` | Kanata doesn't detect OS input sources; it handles remapping directly | Separate `.kbd` files per layout, or use Kanata layers |
| **Mouse cursor movement** (caps-mode arrow → mouse move) | `{:mkey {:x …}}` / `{:pkey :button1}` | Kanata has limited mouse emulation (`movemouse*` actions); requires `danger-enable-cmd yes` on some platforms | Use `movemouse-accel` in Kanata if mouse control is desired |
| **ISO angle mod** (z on grave key, shifted bottom-left row) | Colemak DH ISO-extended input source | ANSI Colemak DH is used instead for cross-platform consistency | Add a separate ISO layer or adjust `defsrc`/`deflayer` for ISO boards |
| **Colemak DH Matrix variant** (for split keyboards) | `:colemakdhmatrix` input source | Different key mapping for matrix/columnar boards | Create a separate `deflayer` with the matrix variant |
| **One-shot shift** | Commented out in edn (variable-based mode) | Was already disabled in Karabiner config | Kanata has native `one-shot` action if desired |
| **Keyboard Maestro integration** | `:km "macro name"` template | macOS-only, requires Keyboard Maestro | Use `(cmd ...)` or OS-level automation |

---

## Behavioral differences

| Aspect | Karabiner | Kanata |
|---|---|---|
| **Simlayer activation** | Dedicated simlayer engine: press key, if another key follows within threshold → layer activates; key alone → original character | `tap-hold-press`: similar result but all taps have a small delay equal to `$tt` before the character emits |
| **Simultaneous combos** | Built-in `simultaneous` from-event with configurable threshold | `defchordsv2`: all chord member keys are held until the chord timeout expires; may add slight latency to those keys during normal typing |
| **Modifier passthrough (##)** | `##` means any additional modifiers pass through to the output | In Kanata, external modifier state is naturally preserved through layers — arrows in nav layer work with Shift for selection, etc. |
| **Notifications** | Karabiner can show macOS notifications (`:noti`) | No equivalent in Kanata |
| **Per-app conditions** | `:applications` conditions (`:terminals`, `:vscode`, etc.) | Kanata has `defoverrides` which can match process names on some platforms, but support is limited |
