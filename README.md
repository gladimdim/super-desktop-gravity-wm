# Center Magnify

A [SUPER DESKTOP](https://github.com/gladimdim/super-desktop) plugin that
changes how cards are laid out:

- The closer a card is to the **horizontal centre** of the screen, the
  **larger** it is drawn: up to **70%** of the screen width.
- Move it toward the left or right and it **shrinks** smoothly.
- At the **edges** it becomes an **icon** docked at that edge. Drag an icon
  toward the centre and it grows back into a card; drop it there and it opens.

Every change animates. A card keeps its own terminal size: it is only drawn
larger or smaller, so its columns and rows never change.

## Turn it on and off

- The **🔍 Magnify** button in the top bar (drawn pressed while it is on), or
  **SUPER + ALT + M** while SUPER DESKTOP is shown, turns the effect off and on.
  Your choice is remembered.
- Turning the plugin off in **Settings → Plugins** removes it completely.

With the effect off, every card goes back to where you put it. Center Magnify
never moves your saved layout, with one exception: a card you **drop** in an
edge band is saved as an icon there, and an icon you drop near the centre is
saved open, just as if you had minimized or opened it yourself.

## Settings

Settings → Plugins → Center Magnify:

| Setting | Default | |
| --- | --- | --- |
| Width at the centre | 70% | How wide a card is drawn at the centre of the screen (20–95). |
| Width near the edges | 22% | How wide it is just before it becomes an icon (5–60). |
| Icon band at each side | 12% | How close to an edge, as a share of half the screen, a card becomes an icon (2–40). |
| Icon size | 72 px | 48–160. |
| Animation smoothing | 90 ms | Higher is softer and slower; 0 jumps. |

## Install

Needs a SUPER DESKTOP build with plugin API 1 (`super-desktop plugin describe`
says so). Only one window renderer can be on at a time.

```sh
git clone https://github.com/gladimdim/super-desktop-center-magnify
super-desktop plugin link super-desktop-center-magnify
super-desktop plugin activate center-magnify
```

`renderer.wasm` is committed, so nothing needs building. To remove it:
`super-desktop plugin deactivate center-magnify`, or its switch in
⚙ Settings → Plugins.

## Permissions

| Permission | Why |
| --- | --- |
| `layout.renderer` | Decide where and how big cards are drawn. |
| `ui.toolbar` | The 🔍 Magnify on/off button. |
| `shortcuts.overlay` | SUPER + ALT + M inside SUPER DESKTOP. |

Center Magnify has **no process**: it is a WebAssembly module (about 8 KB)
that SUPER DESKTOP runs in a sandboxed interpreter. It cannot read files, use
the network or see what your terminals show; it only gets each card's
rectangle and state and returns where to draw it.

## Build and test

```sh
rustup target add wasm32-unknown-unknown
./build.sh
```

`build.sh` runs the native tests, builds `renderer.wasm`, then
`super-desktop plugin validate .` and `super-desktop plugin test .`: a
conformance run (0 to 128 cards on three screen sizes, a drag and a drop,
within the frame budget) and the behaviour scenarios in `tests/`.

## License

MIT
