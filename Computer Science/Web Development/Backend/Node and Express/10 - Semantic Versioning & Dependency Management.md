# 10 - Semantic Versioning (SemVer) & Dependency Management

*(Following Piyush Garg's Node.js playlist.)*

## What is it?

**Semantic Versioning (SemVer)** is a versioning convention almost every npm package follows: `MAJOR.MINOR.PATCH` (e.g. `4.18.2`). Each number changing signals a specific kind of change to anyone depending on that package — it's a shared agreement across the entire JS ecosystem, not something Node enforces by force.

```
4  .  18  .  2
▲     ▲     ▲
MAJOR MINOR PATCH
```

## Why does it exist?

When you depend on a third-party package (see [[03 - npm, package.json & Node Modules]]), that package's author will keep releasing new versions — bug fixes, new features, occasionally changes that break how existing code uses it. **Without a shared convention, you'd have no way to know from the version number alone whether upgrading is safe.** SemVer solves this by making the version number itself communicate intent:

| Segment | Meaning | Example |
|---|---|---|
| **MAJOR** | **Breaking changes.** Existing code using the package may stop working after this update — function signatures changed, behavior changed, things removed. | `4.x.x → 5.0.0` |
| **MINOR** | **New features, backward-compatible.** Existing code keeps working exactly as before; new capabilities are simply added on top. | `4.18.x → 4.19.0` |
| **PATCH** | **Bug fixes only, backward-compatible.** No new features, no breaking changes — just fixes. | `4.18.2 → 4.18.3` |

**The core risk to internalize:** a MAJOR bump is the only one that's *allowed* to break your code under the SemVer convention. MINOR and PATCH updates are supposed to be safe to accept automatically — which is exactly what the version symbols below are designed around.

## Versioning symbols in `package.json`

When you install a package, npm records a version range in `package.json` using a symbol prefix — this controls what `npm install`/`npm update` is allowed to automatically upgrade to later.

### Caret `^` — the default

```json
"dependencies": {
  "express": "^4.18.2"
}
```
`^4.18.2` allows any version that is **`>= 4.18.2` and `< 5.0.0`** — i.e., MINOR and PATCH updates are permitted automatically, but the MAJOR version is locked. This is npm's default when you run `npm install <package>`, based on the assumption that MINOR/PATCH updates are safe per SemVer's own rules.

### Tilde `~` — more conservative

```json
"dependencies": {
  "express": "~4.18.2"
}
```
`~4.18.2` allows only **PATCH** updates — i.e., `>= 4.18.2` and `< 4.19.0`. MINOR updates (new features) are blocked too, not just MAJOR. Use this when you want maximum stability and are only willing to accept bug fixes automatically, not even new (backward-compatible) features.

### No symbol — exact lock

```json
"dependencies": {
  "express": "4.18.2"
}
```
No symbol means **exactly** that version, nothing else — `npm install`/`npm update` won't move it at all unless you explicitly change the number yourself or run `npm install express@latest`.

| Symbol | Allows | Blocks |
|---|---|---|
| `^4.18.2` | Minor + patch updates (`4.19.0`, `4.99.9`, etc.) | Major updates (`5.0.0`) |
| `~4.18.2` | Patch updates only (`4.18.3`, `4.18.9`) | Minor and major updates |
| `4.18.2` (no symbol) | Nothing automatically | Everything — fully locked |

## Best practices

- **Read the changelog before upgrading**, especially across a MAJOR version — SemVer is a *convention*, not a guarantee; maintainers can and occasionally do make mistakes, and even "safe" MINOR/PATCH updates are worth a quick skim if the package is critical to your app.
- **Check for security fixes specifically** — `npm audit` reports known vulnerabilities in your installed dependencies; a PATCH release addressing a security issue is usually worth updating for promptly, even in a stable project.
- **Avoid installing with the `latest` tag** (`npm install express@latest`) as a habit — this ignores your version range entirely and can jump you straight to a new MAJOR version without warning, defeating the whole purpose of the caret/tilde constraints you'd otherwise rely on.
- **Manually pin/lock a version** when a package has proven unstable, when you need long-term reproducibility, or when you've deliberately decided not to upgrade yet — done by removing the `^`/`~` prefix in `package.json`, or more robustly via the lockfile (below).

## Manual control: locking versions precisely

Beyond editing `package.json` directly, npm also generates a **`package-lock.json`** file automatically — this records the *exact* resolved version of every package (including nested dependencies-of-dependencies) actually installed at a point in time. `package.json` states your allowed *range*; `package-lock.json` freezes the *exact* versions that satisfied that range on your last install. Committing `package-lock.json` to version control ensures teammates (and production deployments) get byte-identical dependency versions, even though `package.json` itself only specifies ranges.

```bash
npm install express@4.17.0     # install and lock to this exact version in package.json
npm install express@latest     # ⚠️ jumps to newest version, ignoring your existing range — use deliberately, not as a habit
```

## Common mistakes

- Assuming `^` is "always safe" — it's safe *according to the SemVer convention*, but a poorly-versioned package can still ship a breaking change inside what it labels a MINOR release. Convention isn't enforcement.
- Running `npm install <package>@latest` out of habit to "get the newest version," not realizing this silently jumps across MAJOR versions and can break the app.
- Never committing `package-lock.json` — without it, two different installs (yours vs a teammate's vs production) can quietly resolve to different transitive dependency versions, causing "works on my machine" bugs.
- Confusing `~` and `^` — remembering it as "tilde is tighter" (only patches) vs "caret is more permissive" (minor + patch) helps keep them straight.

## Related concepts
[[03 - npm, package.json & Node Modules]] — where these version strings live and what `dependencies` means
