# Advent of Code 2021 (C++)

Solutions to [Advent of Code 2021](https://adventofcode.com/2021) in C++20, built with CMake.

## Layout

```
.
├── CMakeLists.txt          # builds one executable per day
├── dayNN/
│   ├── src/dayNN.cpp       # solution source
│   └── input/dayNN.txt     # puzzle input
└── .devcontainer/          # optional VS Code dev container (see below)
```

Each `dayNN/src/dayNN.cpp` is auto-discovered and compiled into its own
executable named `dayNN` (C++20, `-Wall -Wextra -Wpedantic`, Debug by default).

## Build on WSL (Ubuntu 26.04)

The project lives on the Windows filesystem but is built from inside WSL. Open
the WSL shell (`wsl` from PowerShell, or the "Ubuntu" terminal) and `cd` into
the repo — from Windows it's reachable at `/mnt/c/Users/aranz/Desktop/AdventOfCode2021`.

A fresh Ubuntu 26.04 install has no C++ toolchain, so install it once:

```bash
sudo apt update
sudo apt install -y build-essential cmake gdb
```

This pulls in g++/gcc 15.2, CMake 4.2, and gdb 17.1 — everything the project
needs. Verify with:

```bash
cmake --version && g++ --version
```

> Tip: build into a WSL-local path (e.g. `~/aoc-build`) rather than a `build/`
> dir on `/mnt/c`. Compiling across the Windows↔Linux filesystem boundary is
> noticeably slower. The commands below use `-B build` for simplicity; swap in
> `-B ~/aoc-build` if you want the speed-up.

## Build & run

```bash
cmake -S . -B build          # configure
cmake --build build          # compile all days
./build/day01                # run (reads day01/input/day01.txt)
```

Run a day against a specific input file:

```bash
./build/day01 path/to/input.txt
```

## Adding a new day

```bash
mkdir -p day02/src day02/input
touch day02/src/day02.cpp day02/input/day02.txt
cmake -S . -B build          # re-glob picks up the new source
cmake --build build
```

## Optional: VS Code dev container

Prefer a pinned, reproducible toolchain over the WSL one? Open the folder in
VS Code and *Reopen in Container* to use `.devcontainer/` instead — no local
install needed. The WSL path above is the default; the container is just an
alternative.

---

## C++ build & compile cheat sheet

### CMake (this project)

| Command | What it does |
|---|---|
| `cmake -S . -B build` | Configure: generate build system into `build/` |
| `cmake --build build` | Build every target |
| `cmake --build build --target day01` | Build just one day |
| `cmake --build build -j` | Parallel build (all cores) |
| `cmake --build build --clean-first` | Clean then rebuild |
| `rm -rf build && cmake -S . -B build` | Full clean reconfigure |
| `cmake -S . -B build -DCMAKE_BUILD_TYPE=Release` | Optimized release build |

> `compile_commands.json` is exported into `build/` for clangd / IntelliSense.

### Build types

| Type | Flags (gcc) | Use |
|---|---|---|
| `Debug` (default) | `-g` | Development, debugging |
| `Release` | `-O3 -DNDEBUG` | Fastest runtime |
| `RelWithDebInfo` | `-O2 -g` | Optimized + debuggable |

### g++ directly (no CMake)

```bash
# Compile + link in one step (C++20, warnings on)
g++ -std=c++20 -Wall -Wextra -Wpedantic -O2 -o day01 day01/src/day01.cpp

# Separate compile and link
g++ -std=c++20 -c day01/src/day01.cpp -o day01.o   # -> object file
g++ day01.o -o day01                                # link

# Debug build for gdb
g++ -std=c++20 -g -O0 -o day01 day01/src/day01.cpp
```

| Flag | Meaning |
|---|---|
| `-std=c++20` | Language standard |
| `-c` | Compile to object file, don't link |
| `-o <file>` | Output name |
| `-O0/-O2/-O3` | Optimization level (none → aggressive) |
| `-g` | Emit debug symbols |
| `-Wall -Wextra -Wpedantic` | Enable broad warnings |
| `-fsanitize=address,undefined` | ASan/UBSan runtime checks |
| `-I <dir>` | Add header search path |

### Debug & inspect

```bash
gdb ./build/day01            # debugger
valgrind ./build/day01       # memory check
./build/day01 < input.txt    # feed stdin instead of file arg
```
