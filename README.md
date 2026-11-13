# QMx2040 ZMK Module

A Zephyr module containing the ZMK firmware for the [QMx2040] and the Quaxe.

[QMx2040]: https://onedeadkey.github.io/qmx2040/

Contrary to most other ergonomic keyboards, this firmware is built to be
entirely layout-agnostic. This means it’s fully compatible out-of-the-box with
any European keyboard layout, without any adaptations necessary.

## Features

### DIP-Switches

Parts of the keymap can be toggled on / off by sliding one of the dip-switches.
Going from left to right, those switches can:

- enable thumb taps
- enable home row mods
- disable sticky keys
- enable Mac support
- toggle your custom layer

### Symbols (QMx2040)

Space taps Space and holds Navigation after 300 ms, whether thumb taps are
enabled or disabled.

With thumb taps enabled:

- The left Backspace/Nav thumb key only sends Backspace (no layer on hold).
- AltGr taps a one-shot AltGr modifier and holds Symbols after 300 ms.

Release Space or AltGr to leave its layer. Symbols follows the
[Arsenik Symbols layer](https://github.com/OneDeadKey/arsenik#symbols).
With thumb taps disabled, AltGr keeps its original behavior.

The symbol keycodes target **French AZERTY on Linux/Windows**. The following
positions use physical QWERTY labels, as in the keymap, rather than AZERTY
legends:

```text
Q W E R T   Y U I O P       ^ < > $ %   @ & * ' `
A S D F G   H J K L ;       { ( ) } =   \ + - / "
Z X C V B   N M , . /       ~ [ ] _ #   | ! ; : ?
```

The existing navigation thumb key remains available. On hosts where AltGr+2
or AltGr+7 are dead keys, finish `~` or backtick with Space.

### Navigation (QMx2040)

Navigation follows the orange bindings in the reference layout. Positions below
use physical QWERTY labels:

```text
Q: Escape    W: Home  E: Up    R: End    T: Page Up
A: Select all S: Left D: Down  F: Right G: Page Down
Z: Undo      X: Cut   C: Copy  V: Paste B: Redo

U I O P       7 8 9 /
H J K L ;     - 4 5 6 0
N M , . /     , 1 2 3 .
```

Editing shortcuts target French AZERTY and use Command when the Mac DIP switch
is enabled. The number row keeps F1–F12; media and brightness keys remain
available around the main grid.

### Mod-Holds

The navigation layer makes use of a custom zmk behavior called a `mod-hold`.
Any active modifier when pressing the nav key will be held for as long as the
nav key is held. This allows you to easily combine modifiers with this layer,
even if they are on the same hand (like ctrl and shift).

### Numpad Keys

This keymap uses a numpad, but doesn’t include a `num-lock` key, as the state
of this flag is handled by the keyboard. When pressing a numpad key, `num-lock`
will be automatically forced on if it wasn’t already.

## Quaxe

The Quaxe is a variant of the QMx2040 where you split the board and only keep
the left hand. It comes with its own one-handed keymap. It’s particularily
adequate for setups with heavy mouse usage (like when using CAD or
graphic-design software), or if you just hurt your right hand and need to let
it rest (which is how I’m typing this doc btw).

Along with making *heavy use* of the `mod-hold` behavior, it has its own set
of features:

### Mirror Layer

This is the corner stone of the whole keymap. Holding the space key will
activate a layer where every key on the right hand are mirrored onto the left
hand (i.e., you get LKJ instead of SDF, assuming you use a Qwerty/Qwertz/Azerty
keyboard). Using this, in tandem with the next feature, you have access to
every alpha-numeric key on an ISO keyboard.

### Callum-Mods

The two most common ways of handling modifiers are thumb-taps and home row
mods, but neither are viable here (the former is way too space-inefficient, and
the latter is very uncomfortable when performed one-handed), which is why we
use [Callum-mods]. Essencially, modifiers are available as one-shot-mods on a
separate layer, which makes them very safe and easy to type one-handed. A
common downside of this approach is that they can be combersome when combined
with layers, but with mod-holds on every thumb key, this is a very rare issue.

[Callum-mods]: https://github.com/qmk/qmk_firmware/blob/user-keymaps-still-present/users/callum/readme.md#oneshot-modifiers

### Fancy Tap-Dances

In order to fit *all* of the keymap on five thumb keys and some half-layers,
the quaxe makes heavy use of tap-dances. Most thumb keys also act as a mirror
when tap-and-held, which is how you get access to the numpad or AltGr with
keys on the right hand.

However these aren’t regular tap-dances, as this keymap depends on a [ZMK
fork][fork], which spices up tap-dances with a couple of new features:

- fixes a bug where nested hold taps weren’t interrupted by other key presses
  properly (though with a regression on tap-dances nested in combos)
- adds a feature to allow processing each binding immediatly, instead of
  waiting for the tapping term (which is used to carry a one-shot-shift into
  mirror for efficient typing)
- adds a feature to allow continuing a tap-dance if you press it again quickly
  after releasing it, no matter how long the key was held for (used to cycle
  between AltGr and Mirror + AltGr, or carry callum-mods into mirror)

Hopefully, these features will one day make it into upstream ZMK, once they are
sufficiently polished.

[fork]: https://github.com/nuclear-Squid/zmk
