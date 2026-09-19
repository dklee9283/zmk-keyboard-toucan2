# Toucan 2 locked layout (36-key)

QWERTY letter positions are unchanged (no Miryoku alpha rearrange). This fork
targets the **36-key** Toucan 2 (3×5 + thumbs): outermost pinky columns are
dropped in the matrix transform. Mac is the default OS. Firmware:
`config/toucan.keymap` (kept in sync with `boards/shields/toucan/toucan.keymap`).
Trackpad: Azoteq TPS43 on the right half (`boards/shields/toucan/toucan_right.overlay`).

## Thumbs (left → right)

`WIN toggle` · `NAV` (layer-tap) · `Space` | `Enter` · `SYM` (layer-tap) · `FUN` (hold)

- **WIN**: tap to toggle Windows LVDI chord mode (sticky). Display shows `WIN`.
- **NAV**: tap = `Tab`, hold = NAV layer.
- **SYM**: tap = `Backspace`, hold = SYM layer.
- **FUN**: momentary hold.
- **ADJ**: hold NAV and SYM together (tri-layer / conditional `NAV+SYM`).

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
thumb). `KEYS_L` / `KEYS_R` / `THUMBS` cover positions `0–35`.

## Matrix (36-key)

Upstream `toucan.dtsi` is 42-key (6+6 columns). This fork’s `default_transform`
drops outermost pinky columns `RC(*,0)` and `RC(*,11)`, keeping:

```
RC(0,1)..RC(0,5)  RC(0,6)..RC(0,10)
RC(1,1)..RC(1,5)  RC(1,6)..RC(1,10)
RC(2,1)..RC(2,5)  RC(2,6)..RC(2,10)
RC(3,3) RC(3,4) RC(3,5)  RC(3,6) RC(3,7) RC(3,8)
```

Physical layout attrs match those 36 positions (outer pinkies removed).

## Layers

### BASE

```
 Q   W   E   R   T        Y   U   I   O   P
 A*  S*  D*  F*  G        H   J*  K*  L*  ;*
 Z   X   C   V   B        N   M   ,   .   /
      WIN  NAV  SPC     RET  SYM  FUN
```

`*` = home-row mod. NAV tap=`Tab`, SYM tap=`BSPC`.

### NAV (hold left inner thumb)

```
 1   2   3   4   5        6   7   8   9   0
 ESC HOME PGDN PGUP END  LEFT DOWN UP RIGHT BSPC
 Undo Cut Copy Paste Redo  PSp Comment ⌥← ⌥→ '
      WIN  NAV  SPC     RET  SYM  FUN
```

Mac chords: Undo `Cmd+Z`, Cut `Cmd+X`, Copy `Cmd+C`, Paste `Cmd+V`,
Redo `Cmd+Shift+Z`, Paste Special `Cmd+Ctrl+V` (Excel Mac), Comment `Shift+F2`,
word-jump `Option+←/→`.

### SYM (hold right inner thumb)

```
 !   @   #   $   %        ^   &   *   (   )
 Find Repl Bord Bold Save  -   =   [   ]   \
 ?   ~   !   &   $        _   +   {   }   |
      WIN  NAV  SPC     RET  SYM  FUN
```

Left home is the Excel pack (Mac): Find `Cmd+F`, Replace `Cmd+H`,
borders `Cmd+Opt+0` (Excel Mac outline border), Bold `Cmd+B`, Save `Cmd+S`.

Left bottom: `? ~ ! & $`. Right home: `- = [ ] \`. Right bottom: `_ + { } |`.
`*` is `ASTRK` (not keypad).

### FUN (hold right outer thumb)

```
 F1  F2  F3  F4  F5       F6  F7  F8  F9  F10
 F8  F4  F2  _   _        F2  F4  F8  F11 F12
 F1  F3  F5  F7  F9       F10 F11 F12 _   _
      WIN  NAV  SPC     RET  SYM  FUN
```

`_` = `&trans`. Home row puts Bloomberg-priority **F8 / F4 / F2** on `A S D`
and `H J K`, with **F11 / F12** on the right home outer columns.

### ADJ (NAV + SYM)

```
 CLR BT0 BT1 BT2 BT3      BT4 VOL- MUTE VOL+ BRI+
 Stu USB BLE  _  WIN      LCLK RCLK MCLK Scr↓ Scr↑
  _   _   _   _   _        _   _   _   _   _
      WIN  NAV  SPC     RET  SYM  FUN
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
