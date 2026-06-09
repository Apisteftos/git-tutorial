# Versioning Guide

## What is Semantic Versioning?

Semantic Versioning (SemVer) is a versioning standard used by most open source projects.
It gives meaning to version numbers so users and contributors understand what changed.

Official spec: [https://semver.org](https://semver.org)

---

## Format

```
MAJOR.MINOR.PATCH
  │     │     │
  │     │     └── bug fixes, no new features, backwards compatible
  │     └──────── new features added, backwards compatible
  └────────────── breaking changes, API changed, not backwards compatible
```

---

## Rules

### PATCH — increment when you fix a bug
```
0.1.0  →  0.1.1   fixed segfault in subplots()
0.1.1  →  0.1.2   fixed memory leak in Axes::plot()
```

### MINOR — increment when you add a new feature
```
0.1.0  →  0.2.0   added hist() to Axes class
0.2.0  →  0.3.0   added legend() free function
```
When you increment MINOR, reset PATCH to 0:
```
0.1.3  →  0.2.0   NOT 0.2.3
```

### MAJOR — increment when you break the API
```
0.x.x  →  1.0.0   stable release, API locked
1.0.0  →  2.0.0   subplots() signature changed
```
When you increment MAJOR, reset MINOR and PATCH to 0:
```
1.3.2  →  2.0.0   NOT 2.3.2
```

---

## Development Phase (0.x.x)

While the version starts with `0`, the API is considered unstable:
```
0.1.0  →  first working version
0.2.0  →  more features, API may still change
0.x.x  →  still in development
```

Once the API is stable and the project is production ready:
```
0.x.x  →  1.0.0   first stable release
```

---

## Version History Example

```
0.1.0   first working version
        Axes, Figure, PyPtr, Interpreter, basic free functions

0.1.1   bug fix
        fixed subplots() segfault with numpy array

0.2.0   new features
        added hist(), boxplot(), legend() to Axes

0.2.1   bug fix
        fixed memory leak in toNumpy()

0.3.0   new features
        added 3D plotting support

1.0.0   stable release
        API locked, production ready

1.1.0   new features
        added animation support

2.0.0   breaking change
        subplots() now returns SubplotsResult instead of std::pair
```

---

## Pre-release Versions

For testing before an official release:
```
1.0.0-alpha    very early, may be unstable
1.0.0-beta     feature complete, being tested
1.0.0-rc.1     release candidate, almost ready
1.0.0          final release
```

---

## How to Tag a Release on GitHub

```bash
# tag locally
git tag -a v0.1.0 -m "First release — Axes, Figure, basic free functions"

# push tag to GitHub
git push origin v0.1.0

# push all tags at once
git push origin --tags
```

Then on GitHub:
```
Releases → Draft a new release → choose tag → add release notes → Publish
```

---

## What Goes in Each Release

A good release on GitHub should include:

```markdown
## What's New in v0.2.0

### Added
- hist() method in Axes class
- legend() free function
- boxplot() method in Axes class

### Fixed
- Memory leak in toNumpy() for large vectors
- segfault when subplots() called with squeeze=True

### Changed
- subplots() now accepts const std::vector<long>&

### Removed
- Nothing removed in this release
```

---

## Never Do This

```
# wrong — skipping versions
0.1.0  →  0.5.0   skipped 0.2.0, 0.3.0, 0.4.0

# wrong — going backwards
1.0.0  →  0.9.0

# wrong — adding features in a PATCH
0.1.0  →  0.1.1   but added new Axes methods (should be 0.2.0)

# wrong — breaking API in a MINOR
0.1.0  →  0.2.0   but changed subplots() signature (should be 1.0.0 or wait)
```

---

## Quick Reference

| Change type | Example | Version bump |
|-------------|---------|--------------|
| Bug fix | fixed segfault | PATCH |
| New method | added hist() | MINOR |
| New class | added Animation class | MINOR |
| API change | renamed Axes::plot() | MAJOR |
| Removed feature | removed Figure::clf() | MAJOR |
| Documentation only | updated README | no bump needed |
| Internal refactor | no API change | PATCH |
