# Changelog

All notable changes to this project will be documented in this file.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning follows [Semantic Versioning](https://semver.org/).

---

## Version Numbering Guide

```
MAJOR.MINOR.PATCH
  │     │     │
  │     │     └── bug fixes, no new features
  │     └──────── new features, backwards compatible
  └────────────── breaking changes, API changed
```

| Version | Meaning |
|---------|---------|
| 0.x.x | in development, API may change |
| 1.0.0 | stable, production ready |
| 1.1.0 | new features added |
| 2.0.0 | breaking changes |

---

## [Unreleased]

Work in progress — not yet tagged as a release.

### In Progress
- `xlabel()`, `ylabel()` free functions
- `legend()` free function
- `hist()`, `boxplot()` in Axes class
- `annotations()` support
- README documentation

---

## [0.1.0] - 09/06/2026

First working version. Core architecture established.

### Added
- `PyPtr.h` — RAII smart pointer for `PyObject*` using `std::unique_ptr` with custom `PyDecRef` deleter
- `Interpreter.h` — Meyers' singleton managing Python interpreter lifecycle via RAII
- `numpy_utils.h` — `NumpyType<T>` type trait + `toNumpy()` template function converting `std::vector<T>` to numpy array
- `Axes.h` — RAII-based OOP wrapper for matplotlib axes object with methods: `plot()`, `grid()`, `set_title()`, `set_xlabel()`, `set_ylabel()`, `set_xlim()`, `set_ylim()`, `set_aspect()`, `errorbar()`, `scatter()`, `step()`, `loglog()`, `semilogx()`, `semilogy()`, `fill_between()`, `fill_betweenx()`, `bar()`, `barh()`, `bar_label()`, `stem()`, `eventplot()`, `pie()`
- `Figure.h` — RAII-based OOP wrapper for matplotlib figure object with methods: `suptitle()`, `save()`, `tight_layout()`, `clf()`
- `functions.h` — pyplot free functions: `show()`, `plot()`, `tight_layout()`, `clf()`, `cla()`, `close()`, `sca()`, `gca()`, `gcf()`, `get_figlabels()`, `get_fignums()`
- `matplotlib.h` — master include header
- `subplots()` — returns `std::pair<Figure, std::vector<Axes>>`, supports any layout (1x2, 2x2, NxM)
- CMake build system with Python3 and numpy auto-detection
- C++23 support throughout

### Fixed
- `subplot()` — was incorrectly passing floats instead of integers, breaking Python 3.11+
- `tick_params()` — numeric parameters now correctly passed as Python numeric types
- `subplots()` — now correctly handles 1D and 2D numpy array of axes for any layout
- Removed deprecated `Py_SetProgramName` and `PySys_SetArgv` API calls

### Architecture
- Complete rewrite from scratch — not a copy of lava/matplotlib-cpp
- RAII memory management throughout — no manual `Py_DECREF` in user code
- Rule of Zero applied via `std::unique_ptr` with custom deleter
- Split into separate headers for clean OOP separation of concerns
- Lazy function lookup via `PyObject_GetAttrString` instead of storing 50+ function pointers
