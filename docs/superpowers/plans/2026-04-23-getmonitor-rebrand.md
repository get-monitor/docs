# GetMonitor Rebrand Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rename all references from "StatusPageOne" / "GetMonitor" / `getmonitor.io` to "GetMonitor" / `getmonitor.io` across the entire docs repository.

**Architecture:** Three sequential phases — (1) rename image files via `git mv`, (2) run in-place text substitutions across all content and config files, (3) verify no old references remain and commit. All changes are in the `docs/` directory of a Mintlify docs site.

**Tech Stack:** Shell (`git mv`, `perl -pi -e`), Mintlify docs (`.mdx`, `docs.json`)

---

## What Changes vs. What Stays

| String | Before | After |
|--------|--------|-------|
| Site name | `GetMonitor` | `GetMonitor` |
| Main domain | `getmonitor.io` | `getmonitor.io` |
| Console URL | `console.getmonitor.io` | `console.getmonitor.io` |
| X handle | `x.com/get_monitor` | `x.com/get_monitor` |
| GitHub handle | `github.com/get-monitor` | `github.com/get-monitor` |
| LinkedIn handle | `linkedin.com/company/getmonitor` | `linkedin.com/company/getmonitor` |
| Status page subdomain | `*.statuspage.one` | **UNCHANGED** |
| Badge API URLs | `status.statuspage.one/api/badges/...` | **UNCHANGED** |
| Webhook User-Agent | `StatusPageOne-Monitor/1.0` | **UNCHANGED** |

---

## Files Modified

- **Rename (23 image files):** `images/product/console.getmonitor.io_*.png` → `console.getmonitor.io_*.png`, and `getmonitor.io_.png` → `getmonitor.io_.png`
- **Modify:** `docs.json` — site name, URLs, social links, navbar button
- **Modify:** `index.mdx` — brand name in description and body
- **Modify:** `getting-started/introduction.mdx` — brand name, console URL, image references
- **Modify:** `monitoring/monitors-overview.mdx` — brand name, image references
- **Modify:** `monitoring/creating-monitors.mdx` — image references
- **Modify:** `monitoring/monitor-statistics.mdx` — image references
- **Modify:** `monitoring/monitor-alerts.mdx` — image references
- **Modify:** `status-pages/overview.mdx` — brand name, image references
- **Modify:** `status-pages/components.mdx` — example monitor name, image references
- **Modify:** `status-pages/custom-domains.mdx` — brand name, image references (note: `statuspage.one` subdomain refs unchanged)
- **Modify:** `status-pages/badges.mdx` — brand name, image references (note: badge API URLs and `statuspage.one` refs unchanged)
- **Modify:** `notifications/integrations.mdx` — brand name, image references (note: `StatusPageOne-Monitor/1.0` unchanged)
- **Modify:** `notifications/alert-rules.mdx` — image references
- **Modify:** `organization/team-members.mdx` — image references
- **Untouched:** `status.statuspage.one_.png`, `statuspage.one_en-US_status_admin_customize.png`

---

## Task 1: Rename image files

**Files:** `images/product/*.png` (23 files)

- [ ] **Step 1: Rename all `console.getmonitor.io_` images**

Run from `/Users/washington/Projects/GetMonitor/docs`:

```bash
cd /Users/washington/Projects/GetMonitor/docs && \
for f in images/product/console.getmonitor.io_*; do
  new="${f/console.getmonitor.io_/console.getmonitor.io_}"
  git mv "$f" "$new"
done
```

Expected: 22 `git mv` operations, no errors.

- [ ] **Step 2: Rename `getmonitor.io_.png`**

```bash
git mv images/product/getmonitor.io_.png images/product/getmonitor.io_.png
```

Expected: success, no errors.

- [ ] **Step 3: Verify renames**

```bash
git status --short | grep -E "^R"
```

Expected: 23 lines starting with `R` (renamed), all from old to new names. Verify that `status.statuspage.one_.png` and `statuspage.one_en-US_status_admin_customize.png` are NOT in the list.

---

## Task 2: Text substitutions in content files

**Files:** All `.mdx`, `.json`, `.md`, `.svg` files under the repo root.

Run these substitutions in order. Each `perl -pi -e` call is a single in-place replacement pass.

- [ ] **Step 1: Replace main domain** (`getmonitor.io` → `getmonitor.io`)

This single replacement handles the main domain, console URL (`console.getmonitor.io` → `console.getmonitor.io`), image filename references in markdown, and example text in prose.

```bash
find /Users/washington/Projects/GetMonitor/docs -not -path '*/.git/*' \
  \( -name "*.mdx" -o -name "*.json" -o -name "*.md" -o -name "*.svg" \) \
  -exec perl -pi -e 's/statuspageone\.com/getmonitor.io/g' {} +
```

Expected: silent success. Affects `docs.json`, 13 `.mdx` files.

- [ ] **Step 2: Replace brand name** (`GetMonitor` → `GetMonitor`)

```bash
find /Users/washington/Projects/GetMonitor/docs -not -path '*/.git/*' \
  \( -name "*.mdx" -o -name "*.json" -o -name "*.md" -o -name "*.svg" \) \
  -exec perl -pi -e 's/StatusPage\.one/GetMonitor/g' {} +
```

Expected: silent success. Affects `docs.json`, `index.mdx`, `getting-started/introduction.mdx`, `monitoring/monitors-overview.mdx`, `status-pages/custom-domains.mdx`, `status-pages/overview.mdx`, `notifications/integrations.mdx`.

- [ ] **Step 3: Replace X/Twitter social handle**

```bash
find /Users/washington/Projects/GetMonitor/docs -not -path '*/.git/*' \
  \( -name "*.mdx" -o -name "*.json" -o -name "*.md" \) \
  -exec perl -pi -e 's|x\.com/statuspageone|x.com/get_monitor|g' {} +
```

Expected: affects `docs.json` only.

- [ ] **Step 4: Replace GitHub social handle**

```bash
find /Users/washington/Projects/GetMonitor/docs -not -path '*/.git/*' \
  \( -name "*.mdx" -o -name "*.json" -o -name "*.md" \) \
  -exec perl -pi -e 's|github\.com/statuspageone|github.com/get-monitor|g' {} +
```

Expected: affects `docs.json` only.

- [ ] **Step 5: Replace LinkedIn social handle**

```bash
find /Users/washington/Projects/GetMonitor/docs -not -path '*/.git/*' \
  \( -name "*.mdx" -o -name "*.json" -o -name "*.md" \) \
  -exec perl -pi -e 's|linkedin\.com/company/statuspageone|linkedin.com/company/getmonitor|g' {} +
```

Expected: affects `docs.json` only.

---

## Task 3: Verify — no stale references remain

- [ ] **Step 1: Check for any remaining `statuspageone` occurrences**

```bash
grep -rn "statuspageone\|StatusPageOne\|StatusPage\.one" \
  /Users/washington/Projects/GetMonitor/docs \
  --include="*.mdx" --include="*.json" --include="*.md" --include="*.svg" \
  | grep -v ".git"
```

Expected: **zero results.** If any appear, fix them manually before continuing.

- [ ] **Step 2: Confirm protected strings are untouched**

```bash
grep -rn "statuspage\.one\|StatusPageOne-Monitor" \
  /Users/washington/Projects/GetMonitor/docs \
  --include="*.mdx" --include="*.json" --include="*.md" \
  | grep -v ".git"
```

Expected: results exist — badge URLs (`status.statuspage.one/api/badges/...`), subdomain references (`yourcompany.statuspage.one`), and the User-Agent (`StatusPageOne-Monitor/1.0`) must all still be present and unchanged.

- [ ] **Step 3: Confirm image filenames are consistent with references**

```bash
grep -rn "statuspageone\|statuspage\.one" \
  /Users/washington/Projects/GetMonitor/docs/images \
  | grep -v ".git"
```

Expected: only `status.statuspage.one_.png` and `statuspage.one_en-US_status_admin_customize.png` appear (the untouched files).

---

## Task 4: Commit

- [ ] **Step 1: Stage all changes**

```bash
git -C /Users/washington/Projects/GetMonitor/docs add -A
```

- [ ] **Step 2: Review staged diff**

```bash
git -C /Users/washington/Projects/GetMonitor/docs diff --cached --stat
```

Expected: ~23 renamed image files + ~14 modified text files.

- [ ] **Step 3: Commit**

```bash
git -C /Users/washington/Projects/GetMonitor/docs commit -m "$(cat <<'EOF'
chore: rebrand from GetMonitor to GetMonitor

Rename all references from getmonitor.io → getmonitor.io and
GetMonitor → GetMonitor across docs, config, and image filenames.
Social handles updated. statuspage.one subdomains and
StatusPageOne-Monitor/1.0 User-Agent intentionally preserved.

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

Expected: commit succeeds, listing all changed files.
