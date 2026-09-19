# Toucan 2 locked layout

QWERTY letter positions are unchanged (no Miryoku alpha rearrange). Mac is the
default OS. Firmware: `config/toucan.keymap`. Trackpad: Azoteq TPS43 on the
right half (`boards/shields/toucan/toucan_right.overlay`).

## Thumbs (left → right)

`WIN toggle` · `NAV` (hold) · `Space` | `Enter` · `SYM` (hold) · `FUN` (hold)

- **WIN**: tap to toggle Windows LVDI chord mode (sticky). Display shows `WIN`.
- **NAV / SYM / FUN**: momentary layers while held.
- **ADJ**: hold NAV and SYM together (tri-layer).

Physical mouse buttons are not on BASE thumbs. Pointing is the trackpad
(tap / two-finger-tap / press-and-hold / two-finger scroll). ADJ has a tiny
LCLK / RCLK / scroll fallback.

## Home-row mods (BASE)

Hold = modifier, tap = letter. Mac-primary order as locked:

| Hand  | A / ; | S / L | D / K | F / J |
| ----- | ----- | ----- | ----- | ----- |
| Left  | Ctrl  | Alt   | Gui (Cmd) | Shift |
| Right | Ctrl  | Alt   | Gui (Cmd) | Shift |

Timing is **tap-preferred** with a 300 ms tapping-term, `require-prior-idle-ms`,
and positional hold-tap (mods fire when the next key is on the other hand or a
thumb). Dedicated Ctrl (home outer) and Shift (bottom outer) remain as
timing-free backups.

## Layers

### BASE

```
 TAB   Q   W   E   R   T        Y   U   I   O   P   BSPC
 CTRL  A*  S*  D*  F*  G        H   J*  K*  L*  ;*  '
 SHFT  Z   X   C   V   B        N   M   ,   .   /   ESC
           WIN  NAV  SPC      RET  SYM  FUN
```

`*` = home-row mod.

### NAV (hold left inner thumb)

Stock number row kept. Arrows on the right home row; Option-word-jump on `;` `'`.
Clipboard on ZXCVB muscle memory.

```
 TAB   1   2   3   4   5        6   7   8   9   0   BSPC
 ESC  HOME PGDN PGUP END INS   LEFT DOWN UP RIGHT ⌥←  ⌥→
       Undo Cut Copy Paste Redo  PSp Comment
           WIN  NAV  SPC      RET  SYM  FUN
```

Mac chords: Undo `Cmd+Z`, Cut `Cmd+X`, Copy `Cmd+C`, Paste `Cmd+V`,
Redo `Cmd+Shift+Z`, Paste Special `Cmd+Ctrl+V` (Excel Mac), Comment `Shift+F2`.

### SYM (hold right inner thumb)

```
 TAB   !   @   #   $   %        ^   &   *   (   )   BSPC
 Find Repl Bord PSp Bold Save   -   =   [   ]   \   `
  ?    ~   !   &   $   %        _   +   {   }   |   ~
           WIN  NAV  SPC      RET  SYM  FUN
```

Left home is the Excel pack (Mac): Find `Cmd+F`, Replace `Cmd+H`,
Borders `Cmd+Opt+0` (Excel Mac outline border; Format Cells is `Cmd+1` if you
need the full dialog), Paste Special `Cmd+Ctrl+V`, Bold `Cmd+B`, Save `Cmd+S`.

Left bottom duplicates the punctuation Dae hits often (`? ~ ! & $ %`). Right
side is the usual `- = [ ] \` / `_ + { } | ~` map. `*` is `ASTRK` (not keypad).

### FUN (hold right outer thumb)

F-keys are first-class here, not buried on ADJ.

```
 F1  F2  F3  F4  F5  F6       F7  F8  F9  F10 F11 F12
     F8  F4  F2               F2  F4  F8
     F1  F3  F5  F7  F9       F10 F11 F12
           WIN  NAV  SPC      RET  SYM  FUN
```

Home row puts Bloomberg-priority **F8 / F4 / F2** on `S D F` and `J K L`.

### ADJ (NAV + SYM)

```
 CLR  BT0 BT1 BT2 BT3 BT4     VOL- MUTE VOL+ BRI- BRI+ BSPC
 Stu  USB BLE         WIN     LCLK RCLK MCLK Scr↓ Scr↑
           WIN  NAV  SPC      RET  SYM  FUN
```

- **BT0** = Mac, **BT1** = Windows LVDI (also BT2–BT4 if you need more hosts).
- **Stu** = ZMK Studio unlock (left half, USB).
- Mouse keys are fallback only.

### WIN toggle

Tap the left outer thumb. The display name becomes `WIN`. NAV/SYM letter keys
stay the same; the **edit and Excel macros** swap `LG` (Cmd) → `LC` (Ctrl) via
conditional layers `NAV-W` / `SYM-W`:

| Action        | Mac (default)   | WIN toggle          |
| ------------- | --------------- | ------------------- |
| Undo / Cut / Copy / Paste | Cmd+Z/X/C/V | Ctrl+Z/X/C/V |
| Redo          | Cmd+Shift+Z     | Ctrl+Shift+Z        |
| Paste Special | Cmd+Ctrl+V      | Ctrl+Alt+V          |
| Word-jump     | Option+←/→      | Ctrl+←/→            |
| Find / Replace / Bold / Save | Cmd+F/H/B/S | Ctrl+F/H/B/S |
| Borders       | Cmd+Opt+0       | Ctrl+&              |

Tap WIN again to return to Mac. Pair **Bluetooth profile 0** with the Mac and
**profile 1** with the Windows LVDI machine, then select them on ADJ (top row).

Pinch-to-zoom and three-finger swipe OS shortcuts are still compile-time
(`TOUCAN_WIN_MODE` in `toucan.dtsi`, off for Mac-primary). They are independent
of the runtime WIN toggle.

## Trackpad (Azoteq TPS43)

Enabled in `toucan_right.overlay` (driver booleans):

| Gesture         | Result            |
| --------------- | ----------------- |
| Single tap      | Left click        |
| Two-finger tap  | Right click       |
| Press-and-hold  | Click and drag    |
| Two-finger move | Scroll            |

While NAV or SYM is held, one-finger move is also mapped to scroll (existing
input-listener `scroller`).

## Flash notes

1. Enable **GitHub Actions** on this fork (Settings → Actions). The workflow
   is `.github/workflows/build.yml` → `zmkfirmware/zmk` user-config reusable
   workflow. Modules in `config/west.yml` are public; **no `read:org` scope
   and no extra Actions secrets** are required. If a run is skipped, Actions
   were never enabled on the fork.
2. Download the `toucan_left` and `toucan_right` UF2 artifacts from the
   successful run (`settings_reset` is also built if a half will not pair).
3. Double-tap reset on each Seeed XIAO BLE, copy the matching UF2, wait for
   reboot.
