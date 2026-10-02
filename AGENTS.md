# Agent instructions

This is a SUPER DESKTOP plugin (`gravity-wm`): a WebAssembly window
renderer with no process. Follow the `super-desktop-plugin` skill in
`.agents/skills/super-desktop-plugin/SKILL.md` (also at
https://github.com/gladimdim/super-desktop/tree/master/skills/super-desktop-plugin),
above all `references/renderer-abi.md`; review with
`super-desktop-plugin-review` before a release.

- Start with `super-desktop plugin describe --json`: use only what it lists.
- The renderer is `src/lib.rs` (Rust, `no_std` on wasm32). Change the pure
  `present` function and its native tests together.
- `./build.sh` runs `cargo test`, builds `renderer.wasm`, then
  `super-desktop plugin validate .` and `super-desktop plugin test .` (the
  built-in renderer conformance run and the scenarios in `tests/`). All must
  pass before committing. Commit the rebuilt `renderer.wasm`: installation does
  not build anything.
- Settings reach the renderer as `renderer.params`, in manifest order. Keep
  that order and every setting key stable; range-check each value in
  `Tuning::from_params`.
- Keep every frame linear in the card count: 128 cards must stay well inside
  the fuel budget that `plugin test` reports.
