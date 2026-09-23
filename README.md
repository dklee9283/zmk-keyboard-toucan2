# ZMK config for beekeeb Toucan2 Keyboard

[The beekeeb Toucan2 Keyboard](https://beekeeb.com/introducing-toucan2/) is a wireless split column‑stagger keyboard with a display and a trackpad. This fork targets the **locked 36-key** layout (outer pinky columns dropped); upstream is 42-key.

# Customizations

Locked Dae **36-key hybrid** layout (QWERTY, Mac-primary HRM, SYM=punctuation / FUN=F-keys+Excel, NAV ZXCVB clipboard, WIN→WIN_NAV/WIN_FUN, TPS43 gestures): see **[LAYOUT.md](LAYOUT.md)**.

- **Keymap**: [config/toucan.keymap](config/toucan.keymap)
- **General configs**: [boards/shields/toucan/toucan_left.conf](boards/shields/toucan/toucan_left.conf) and [boards/shields/toucan/toucan_right.conf](boards/shields/toucan/toucan_right.conf)
- **Swipe shortcuts**: the `swipe_button_mapper` node in [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi)
- **Invert scroll / trackpad settings**: the `tps43_trackpad` node in [boards/shields/toucan/toucan_right.overlay](boards/shields/toucan/toucan_right.overlay)

Bluetooth: **Profile 0 = Mac**, **Profile 1 = Windows LVDI**. Tap the left outer thumb to toggle WIN mode (Cmd-style chords → Ctrl on NAV/FUN) when using the Windows host. NAV is layer-tap Tab; SYM is layer-tap Backspace; Excel macros live on FUN.

# License

The code in this repo is available under the MIT license.

The included shield nice_view_gem is modified from https://github.com/M165437/nice-view-gem licensed under the MIT License.

The linked trackpad module is based on https://github.com/geeksville/zmk_driver_azoteq

ZMK code snippets are taken from the ZMK documentation under the MIT license.

The embedded font QuinqueFive is designed by GGBotNet, licensed under under the SIL Open Font License, Version 1.1.
