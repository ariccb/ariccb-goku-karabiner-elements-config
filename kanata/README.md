# Kanata — Colemak DH Configuration

A cross-platform [Kanata](https://github.com/jtroo/kanata) keyboard configuration ported from a Karabiner-Elements / Goku setup. It provides Colemak DH letter remapping plus layers for symbols, navigation, and code-snippet macros.

**OS keyboard layout must be set to US QWERTY.** Kanata intercepts at the hardware level and handles all remapping — using a Colemak DH OS input source on top would double-remap keys.

---

## Quick start

### macOS

#### 1. Install Kanata

```bash
# Homebrew
brew install kanata

# — or build from source —
cargo install kanata
```

#### 2. Grant permissions

Kanata needs low-level keyboard access:

| Permission | Where to grant |
|---|---|
| **Input Monitoring** | System Settings → Privacy & Security → Input Monitoring → add the `kanata` binary |
| **Accessibility** | System Settings → Privacy & Security → Accessibility → add the `kanata` binary |

> If you installed via Homebrew the binary is typically at  
> `$(brew --prefix)/bin/kanata` or `/opt/homebrew/bin/kanata`.

#### 3. Disable Karabiner-Elements (if installed)

Kanata and Karabiner both grab the keyboard at the same level. Running both simultaneously causes conflicts — keystrokes may be swallowed or duplicated.

- Open **Karabiner-Elements Preferences → Misc → Uninstall**.
- Or simply quit Karabiner-Elements before starting Kanata.
- The two can coexist on disk; just don't run them at the same time.

#### 4. Run Kanata

```bash
# Foreground (see log output)
sudo kanata -c kanata/kanata.kbd

# Background (launchd plist recommended for auto-start)
sudo kanata -c kanata/kanata.kbd &
```

`sudo` is required on macOS for the IOKit keyboard grab.

#### 5. Auto-start on login (optional)

Create `~/Library/LaunchAgents/com.kanata.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.kanata</string>
  <key>ProgramArguments</key>
  <array>
    <string>/opt/homebrew/bin/kanata</string>
    <string>-c</string>
    <string>/path/to/kanata/kanata.kbd</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
  <key>StandardOutPath</key>
  <string>/tmp/kanata.log</string>
  <key>StandardErrorPath</key>
  <string>/tmp/kanata.err</string>
</dict>
</plist>
```

Load it:

```bash
launchctl load ~/Library/LaunchAgents/com.kanata.plist
```

---

### Windows

#### 1. Install Kanata

Download a release binary from <https://github.com/jtroo/kanata/releases>.  
Pick `kanata.exe` (the default build includes the interception driver).

Or build from source:

```powershell
cargo install kanata
```

#### 2. Install the Interception driver (recommended)

Kanata works best with the [Interception](https://github.com/oblitum/Interception) driver for reliable low-level keyboard interception.

```powershell
# Download and run the Interception installer (requires admin + reboot)
# https://github.com/oblitum/Interception#installation
```

Without Interception, Kanata falls back to a Windows hook which may not capture all keys or work in elevated windows.

#### 3. Switch to the Windows defcfg

Open `kanata.kbd` and swap the active `defcfg` block:

```lisp
;; Comment out the macOS block:
;; (defcfg
;;   process-unmapped-keys yes
;;   concurrent-tap-hold yes
;; )

;; Uncomment the Windows block:
(defcfg
  process-unmapped-keys yes
  concurrent-tap-hold yes
)
```

#### 4. Adjust shortcut modifiers

The default shortcut aliases use `M-` (Meta / Cmd). On Windows, standard shortcuts use Ctrl. Search the `defalias` section for the comment block starting with `macOS defaults` and change:

| macOS | Windows |
|---|---|
| `M-z` | `C-z` |
| `M-S-z` | `C-S-z` (or `C-y`) |
| `M-x` | `C-x` |
| `M-c` | `C-c` |
| `M-v` | `C-v` |
| `M-S-A-v` | `C-S-v` |
| `M-a` | `C-a` |
| `M-tab` | `A-tab` |
| `M-S-tab` | `A-S-tab` |
| `M-S-lbrc` | `C-S-tab` (prev tab) |

Also update chord outputs (`@undo`, `@redo`, etc.) if you change the aliases — the aliases propagate automatically.

#### 5. Run Kanata

```powershell
# From an Administrator terminal:
kanata.exe -c kanata\kanata.kbd
```

#### 6. Auto-start on login (optional)

Use Task Scheduler to create a task that runs `kanata.exe -c <path>\kanata.kbd` at logon with highest privileges.

---

## File overview

| File | Purpose |
|---|---|
| `kanata.kbd` | Main Kanata config — layers, tap-holds, chords, macros |
| `README.md` | This file — install & run on macOS + Windows |
| `MAPPING.md` | Feature-by-feature mapping from Karabiner → Kanata |
| `colemak-dh-notes.md` | How the Colemak DH base layer maps physical keys |

---

## Customisation tips

- **Timing:** Adjust `$tt` (tap timeout) and `$ht` (hold timeout) in `defvar` if tap-holds feel too sluggish or too hair-trigger. Start with 150–250 ms.
- **Chord timeout:** `$ct` controls how tight the simultaneous-press window is. 50 ms is snappy; raise to 80–100 if chords misfire.
- **Email macros:** Replace the `@eml1`–`@eml4` aliases with your own addresses (or delete them).
- **Leader macros:** The `.+key` snippets assume VS Code on macOS. Adjust arrow/word-jump shortcuts (`M-rght` → `C-rght`, `A-rght` → `C-rght`, etc.) for Windows or other editors.
- **App launching:** Kanata can run shell commands with `(cmd ...)` if `danger-enable-cmd yes` is set in `defcfg`. See the Kanata docs for examples.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| Keys not remapped at all | Check that Kanata is running (`ps aux \| grep kanata`). On macOS confirm Input Monitoring permission. |
| Double letters / ghost keys | Karabiner-Elements is probably still running. Quit it first. |
| Chords fire when you don't want them | Increase `$ct` (chord timeout) or remove the offending chord from `defchordsv2`. |
| Tap-hold feels laggy | Decrease `$tt` / `$ht`. Values below 150 ms make taps snappier but holds harder to trigger. |
| "Permission denied" on macOS | Run with `sudo`. Kanata needs root for the IOKit grab. |
| Kanata won't start on Windows | Make sure Interception driver is installed and you're running as Administrator. |
