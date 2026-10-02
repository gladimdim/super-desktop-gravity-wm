# Gravity WM

A [SUPER DESKTOP](https://github.com/gladimdim/super-desktop) plugin that
lays out cards by where they sit on the screen:

- The closer a card is to the **horizontal centre**, the **larger** it gets:
  up to **70%** of the screen width, its height growing with it.
- Move it toward the left or right and it **shrinks** smoothly.
- At the **edges** it becomes an **icon** docked at that edge. Drag an icon
  toward the centre and it grows back into a card; drop it there and it opens.

Cards are **really resized**, not zoomed: a card near the centre gets more
columns and rows, so its terminal shows more. While cards move, the change
animates; the terminal is resized once they come to rest, not on every frame.

## Turn it on and off

- The **🪐 Gravity** button in the top bar (drawn pressed while it is on), or
  **SUPER + ALT + M** while SUPER DESKTOP is shown, turns the effect off and on.
  Your choice is remembered.
- Turning the plugin off in **Settings → Plugins** removes it completely.

With the effect off, every card goes back to where and how big you made it.
Gravity WM never changes your saved layout, with one exception: a card you
**drop** in an edge band is saved as an icon there, and an icon you drop near
the centre is saved open, just as if you had minimized or opened it yourself.

## Settings

Settings → Plugins → Gravity WM:

| Setting | Default | |
| --- | --- | --- |
| Width at the centre | 70% | How wide a card is at the centre of the screen (20–95). Its height grows in proportion, up to the free height. |
| Width near the edges | 22% | How wide it is just before it becomes an icon (5–60); at least the smallest card size. |
| Icon band at each side | 12% | How close to an edge, as a share of half the screen, a card becomes an icon (2–40). |
| Icon size | 72 px | 48–160. |
| Animation smoothing | 90 ms | Higher is softer and slower; 0 jumps. |

## Install

Needs a SUPER DESKTOP build with plugin API 1 (`super-desktop plugin describe`
says so). Only one window renderer can be on at a time. A SUPER DESKTOP
without real resizing for renderers (output mode 2) zooms the cards instead.

```sh
git clone https://github.com/gladimdim/super-desktop-gravity-wm
super-desktop plugin link super-desktop-gravity-wm
super-desktop plugin activate gravity-wm
```

`renderer.wasm` is committed, so nothing needs building. To remove it:
`super-desktop plugin deactivate gravity-wm`, or its switch in
⚙ Settings → Plugins.

## Permissions

| Permission | Why |
| --- | --- |
| `layout.renderer` | Decide where and how big cards are, and resize them. |
| `ui.toolbar` | The 🪐 Gravity on/off button. |
| `shortcuts.overlay` | SUPER + ALT + M inside SUPER DESKTOP. |

Gravity WM has **no process**: it is a WebAssembly module (about 8 KB)
that SUPER DESKTOP runs in a sandboxed interpreter. It cannot read files, use
the network or see what your terminals show; it only gets each card's
rectangle and state and returns where, how big and in which form to draw it.

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
