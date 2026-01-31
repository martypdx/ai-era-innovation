# Vite & Vitest Compatibility Analysis

Analysis of Azoth's Vite plugin compatibility with current and future Vite versions.

---

## Current Versions

| Project | Vite | Vitest | Notes |
|---------|------|--------|-------|
| **Azoth monorepo** | ^5.2.8 | ^1.4.0 | Root devDependencies |
| **Azoth vite-plugin** | ^5.0.12 | — | peerDependency |
| **wre-dashboard-api** | ^5.0.0 | ^1.0.0 | Current project |

**Good news:** Both projects are already on Vite 5.x. The Azoth plugin's peer dependency (`^5.0.12`) is compatible with the connector's Vite version.

---

## Vite Release Timeline

| Version | Release | Key Changes |
|---------|---------|-------------|
| **5.0** | Nov 2023 | Rollup 4, Node 18+, deprecate CJS API |
| **6.0** | Nov 2024 | Environment API, Node 18/20/22+ (dropped 21) |
| **7.0** | June 2025 | Node 20.19+/22.12+ only, ESM-only distribution, `buildApp` hook |

---

## Question 1: Is the Current Plugin Compatible with Newer Vite?

### Vite 5.x → 5.x (Current State)
✅ **Compatible.** Both are on Vite 5.x. No issues expected.

### Vite 5.x → 6.x
⚠️ **Likely compatible, needs verification.**

The Azoth plugin uses stable plugin hooks:
- `configResolved` — stable
- `resolveId` / `load` / `transform` — stable core hooks
- `transformIndexHtml` — stable

Vite 6's "Environment API" is additive—new capabilities for frameworks, not breaking changes to existing transform-style plugins.

**Risk:** Low. The plugin doesn't use deprecated patterns.

### Vite 5.x → 7.x
⚠️ **Will require Node.js update.**

Vite 7 dropped Node 18 support. Azoth's plugin specifies:
```json
"engines": { "node": "^18.0.0 || >=20" }
```

**Action needed:** Update to `"node": ">=20.19"` when targeting Vite 7.

---

## Question 2: Scope of Changes to Upgrade Azoth Project

### To Vite 6.x (Recommended Near-Term)

**Scope: Small**
- Update `peerDependencies` to `^6.0.0`
- Test plugin functionality
- No API changes expected for this plugin's use case

### To Vite 7.x (Current Latest)

**Scope: Small-Medium**
- Node.js 20.19+ or 22.12+ required
- Update `engines` field
- Update `peerDependencies` to `^7.0.0`
- ESM-only (already ESM, should be fine)
- Test thoroughly

### Plugin Code Review

The plugin (`packages/vite-plugin/index.js`) is well-structured:
- Uses `@rollup/pluginutils` for filtering — widely compatible
- Uses standard Vite hooks — not deprecated
- Virtual module pattern (`\0` prefix) — standard practice
- `transformIndexHtml` for template injection — stable API

**No red flags** in the plugin implementation.

---

## Vitest Status

| Version | Key Features |
|---------|--------------|
| **1.x** | Stable, in-source testing, browser mode (experimental) |
| **2.x** | Improved browser mode, better ESM support, snapshot improvements |

Azoth uses `@vitest/browser` for browser testing. The upgrade path from 1.x → 2.x is generally smooth.

**Rough edges in 1.x that may be improved:**
- Browser mode maturity
- Snapshot serialization
- Happy-dom quirks

---

## Recommendations

### Immediate (for reporting project)
1. **Use current Azoth as-is** — Vite 5.x is compatible
2. **Verify Vite plugin works** with the connector's build setup
3. **No upgrades required** for immediate work

### Near-Term (for Azoth maintenance)
1. Upgrade to Vite 6.x, Vitest 2.x
2. Update peer dependency range
3. Test browser-based tests

### Before June 2025
If staying current matters, plan Vite 7 upgrade:
1. Ensure Node 20+ in CI/dev environments
2. Update engine requirements
3. Test ESM-only distribution compatibility

---

## Action Items

- [ ] Test Azoth plugin with wre-dashboard-api's Vite config
- [ ] Verify template injection works with Apps Script HTML output
- [ ] Consider Vite 6 upgrade for Azoth (non-blocking)
- [ ] Document Node version requirements

---

*Analysis date: 2026-01-14*
