# Why this fork exists

This is a fork of [rlcevg/CircuitAI](https://github.com/rlcevg/CircuitAI),
kept for one narrow purpose: **building the BARb skirmish AI natively on Apple
Silicon** for the unofficial
[Recoil Engine macOS port](https://github.com/benbreen/RecoilEngine-AppleSilicon).

It is **not** a competing fork, a rewrite, or a place to send CircuitAI bug
reports. All AI behaviour, design and credit belong upstream — please report
CircuitAI issues to [rlcevg/CircuitAI](https://github.com/rlcevg/CircuitAI),
and macOS **port** issues to
[the port's tracker](https://github.com/benbreen/RecoilEngine-AppleSilicon/issues).

## What is changed here

Branch **`macos-arm64`** (based on the `barbarian` lineage) carries the macOS
port's build fixes only — no gameplay or AI logic changes:

- `src/circuit/module/EconomyManager.h` — a comparison operator made `const`.
  libc++ (Apple's C++ standard library) hard-errors when `std::sort` is given a
  non-const comparator; MSVC and libstdc++ accept it, which is why this only
  shows up on macOS.
- `CMakeLists.txt` — compiles `as_callfunc_arm64_xcode.S`. The engine's source
  glob collects only `.c`/`.cpp`, so AngelScript's arm64 native-call assembly
  was never built and the link failed on missing `_CallARM64*` symbols.

Both are portability fixes and are candidates to go upstream; the fork exists
so the macOS port has a stable, buildable pin in the meantime — and so that pin
does not depend on a branch that could move or disappear.

## How it is consumed

The port pins this repository as a git submodule at
`AI/Skirmish/BARb`, tracking `macos-arm64`. The port builds it with
`AI_TYPES=NATIVE`; building with `AI_TYPES=NONE` skips it entirely.

## Licence

Unchanged: **GNU GPL v2**, as upstream. See `LICENSE`. Modified files are
identified above and in the commit history.
