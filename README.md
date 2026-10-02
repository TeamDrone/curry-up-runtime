# curry-up-runtime

[MicroHs](https://github.com/augustss/MicroHs) (a Haskell implementation by Lennart Augustsson)
compiled to WebAssembly. Curry Up uses it to compile and run students' Haskell code in the browser.
It is served from jsDelivr:

    https://cdn.jsdelivr.net/gh/TeamDrone/curry-up-runtime@mhs-aaf8759c-a4d27ca6/mhs/

| File | |
|---|---|
| `mhs/mhs-worker.mjs` | Emscripten ES module (`export default createMhs`) |
| `mhs/mhs-worker.wasm` | the MicroHs compiler + runtime, with the `base` package embedded |
| `mhs/VERSION` | upstream MicroHs commit and embedded packages |

## Changes from upstream

Built from MicroHs commit `aaf8759cbe4309e8ef6ac0c9a77f4b5ef361190a`, with these differences from the upstream `web-mhs` build:

- a batch build target for Web Workers: no `-sASYNCIFY` (≈15× faster), modularized ES module,
  one `callMain(["-r", "Main.hs"])` per instance;
- garbage-collector reductions disabled (`GCRED=0`, via a small change to
  `src/runtime/unix/config.h`), because their recursive marking overflows the native stack of
  browser workers.

## License

MicroHs is licensed under the Apache License 2.0; see [LICENSE](LICENSE). The changes above are
also available under the Apache License 2.0.
