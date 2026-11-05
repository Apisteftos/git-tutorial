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
0.1.0  →  0.1.1   fixed crash in parse()
0.1.1  →  0.1.2   fixed memory leak in Widget::render()
```

### MINOR — increment when you add a new feature
```
0.1.0  →  0.2.0   added resize() to Widget class
0.2.0  →  0.3.0   added makeWidget() free function
```
When you increment MINOR, reset PATCH to 0:
```
0.1.3  →  0.2.0   NOT 0.2.3
```

### MAJOR — increment when you break the API
```
0.x.x  →  1.0.0   stable release, API locked
1.0.0  →  2.0.0   parse() signature changed
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
        Widget, Window, Buffer, basic free functions

0.1.1   bug fix
        fixed parse() crash with empty input

0.2.0   new features
        added resize(), rotate(), hide() to Widget

0.2.1   bug fix
        fixed memory leak in Buffer::copy()

0.3.0   new features
        added 3D rendering support

1.0.0   stable release
        API locked, production ready

1.1.0   new features
        added animation support

2.0.0   breaking change
        parse() now returns ParseResult instead of std::pair
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
git tag -a v0.1.0 -m "First release — Widget, Window, basic free functions"

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

A good release on GitHub should include all of these sections when relevant:

```markdown
## What's New in v0.2.0

### Added
- resize() method in Widget class
- makeWidget() free function
- rotate() method in Widget class

### Changed
- parse() now accepts const std::vector<long>&

### Deprecated
- Window::clear() — will be removed in v1.0.0, use Window::reset() instead
- Widget::draw(x, y, flags) — use Widget::draw(DrawConfig) instead

### Removed
- Nothing removed in this release

### Fixed
- Memory leak in Buffer::copy() for large inputs
- crash when parse() is called with an empty string

### Security
- Fixed potential buffer overflow in Buffer::copy() when size=0
```

---

## Changelog Sections Explained

| Section | When to use |
|---------|-------------|
| `### Added` | New features, methods, classes, structs |
| `### Changed` | Changes to existing features that are backwards compatible |
| `### Deprecated` | Features that still work but will be removed in a future version |
| `### Removed` | Features removed in this version |
| `### Fixed` | Bug fixes |
| `### Security` | Security vulnerability fixes |

### Deprecated — features that will be removed soon

Use `### Deprecated` when you want to warn users that something will be removed, but haven't removed it yet:

```markdown
### Deprecated
- `Widget::draw(x, y, flags)` — use `Widget::draw(DrawConfig)` instead, will be removed in v1.0.0
- `Window::clear()` — use `Window::reset()` instead, will be removed in v1.0.0
```

This gives users time to update their code before you remove the feature.

### Security — vulnerability fixes

Use `### Security` when you fix a security vulnerability:

```markdown
### Security
- Fixed buffer overflow in Buffer::copy() when input is empty
- Fixed potential null pointer dereference in Logger::getInstance()
```

For a small C++ library, security fixes are rare but could include:
- Buffer overflows when copying data
- Memory corruption bugs in custom smart pointers
- Null pointer dereferences in calls into C APIs

---

## Never Do This

```
# wrong — skipping versions
0.1.0  →  0.5.0   skipped 0.2.0, 0.3.0, 0.4.0

# wrong — going backwards
1.0.0  →  0.9.0

# wrong — adding features in a PATCH
0.1.0  →  0.1.1   but added new Widget methods (should be 0.2.0)

# wrong — breaking API in a MINOR
0.1.0  →  0.2.0   but changed parse() signature (should be 1.0.0 or wait)
```

---

## Quick Reference

| Change type | Example | Version bump |
|-------------|---------|--------------|
| Bug fix | fixed crash | PATCH |
| New method | added resize() | MINOR |
| New class | added Animation class | MINOR |
| API change | renamed Widget::draw() | MAJOR |
| Removed feature | removed Window::clear() | MAJOR |
| Deprecated feature | marked clear() as deprecated | MINOR |
| Security fix | fixed buffer overflow | PATCH |
| Documentation only | updated README | no bump needed |
| Internal refactor | no API change | PATCH |
