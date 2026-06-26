# GitHub Profile Revamp Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the cliché `medsabbar/medsabbar` GitHub profile README with a lean, distinctive, human-toned version that ships a custom SVG banner and self-hosted stats/streak widgets.

**Architecture:** Three files in this repo — `README.md` (the profile), `assets/banner.svg` (the hero), `.github/workflows/stats.yml` (the regenerator). The workflow runs daily, produces two committed SVGs (`assets/stats.svg`, `assets/streak.svg`), and the README references only those committed assets and the company GitHub link. No third-party hosts.

**Tech Stack:** GitHub-flavored Markdown, hand-authored SVG, GitHub Actions (YAML), `stats-organization/github-readme-stats-action@v2`, `DenverCoder1/github-readme-streak-stats@v1`, `markdownlint-cli` for verification.

---

## File Structure

| Path | Status | Purpose |
|---|---|---|
| `README.md` | Modify (gut & rewrite) | The profile rendered on github.com/medsabbar |
| `assets/banner.svg` | Create | Hand-crafted hero banner, 1280×320, dark + cyan, monospace |
| `assets/stats.svg` | Create (by workflow, then committed) | Self-hosted github-readme-stats card |
| `assets/streak.svg` | Create (by workflow, then committed) | Self-hosted streak card |
| `.github/workflows/stats.yml` | Create | Daily workflow that regenerates the two SVGs |

No new top-level directories. No pinned-projects block.

---

## Task 1: Rewrite `README.md`

**Files:**
- Modify: `README.md` (full rewrite — current 125 lines → target ≤ 45 lines)

- [ ] **Step 1: Replace the entire README content**

Overwrite `README.md` with the following content (exact bytes — no edits before pasting):

```markdown
<div align="center">

![Banner: mohamed sabbar, CTO at IBTIKAR-Technologies — "Building the web, one less-buggy app at a time."](./assets/banner.svg)

**CTO at IBTIKAR-Technologies. Building the web, one less-buggy app at a time.**

</div>

---

I lead engineering at [IBTIKAR-Technologies](https://github.com/IBTIKAR-Technologies),
where we build public-services platforms for Mauritania's government.
Most days I'm in Node.js, Next.js, MongoDB, and Firebase.

**Currently building** — Mauritanian e-government services that citizens
actually want to use.

---

### Day-to-day stack

`Node.js` · `Next.js` · `MongoDB` · `Firebase` · `TypeScript`

---

### Numbers

![Stats](./assets/stats.svg)
![Streak](./assets/streak.svg)
```

- [ ] **Step 2: Verify line count is ≤ 45**

Run: `wc -l README.md`
Expected: output is `<= 45` (e.g. `33 README.md`)

- [ ] **Step 3: Verify no third-party hosts are referenced**

Run: `grep -nE "http://|herokuapp|vercel.app|cyclic" README.md || echo "OK: no third-party hosts"`
Expected: `OK: no third-party hosts`

- [ ] **Step 4: Verify no pinned-projects block exists**

Run: `grep -nE "pinned|Pinned|PINNED|tab=repositories" README.md || echo "OK: no pinned block"`
Expected: `OK: no pinned block`

- [ ] **Step 5: Commit**

```bash
git add README.md
git commit -m "docs(readme): gut and rewrite profile README"
```

---

## Task 2: Author the SVG banner

**Files:**
- Create: `assets/banner.svg`

- [ ] **Step 1: Ensure the assets directory exists**

Run: `mkdir -p assets`

- [ ] **Step 2: Write the SVG file**

Create `assets/banner.svg` with the following content (exact bytes):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1280 320" width="1280" height="320" role="img" aria-label="mohamed sabbar, CTO at IBTIKAR-Technologies — Building the web, one less-buggy app at a time.">
  <defs>
    <style>
      .bg { fill: #0d1117; }
      .rule { stroke: #22d3ee; stroke-width: 1; }
      .name { fill: #e6edf3; font-family: 'JetBrains Mono', Menlo, Consolas, monospace; font-weight: 600; font-size: 64px; letter-spacing: -1px; }
      .handle { fill: #8b949e; font-family: 'JetBrains Mono', Menlo, Consolas, monospace; font-size: 22px; }
      .tagline { fill: #e6edf3; font-family: 'JetBrains Mono', Menlo, Consolas, monospace; font-size: 24px; }
      .footer { fill: #6e7681; font-family: 'JetBrains Mono', Menlo, Consolas, monospace; font-size: 14px; letter-spacing: 1px; }
    </style>
  </defs>

  <rect class="bg" x="0" y="0" width="1280" height="320" />

  <text class="name" x="80" y="140">mohamed sabbar</text>
  <text class="handle" x="80" y="178">@medsabbar</text>

  <line class="rule" x1="384" y1="160" x2="896" y2="160" />

  <text class="tagline" x="640" y="240" text-anchor="middle">Building the web, one less-buggy app at a time.</text>

  <text class="footer" x="640" y="290" text-anchor="middle">Nouakchott, MR  →  Nouakchott, World</text>
</svg>
```

- [ ] **Step 3: Validate the SVG is well-formed XML**

Run: `xmllint --noout assets/banner.svg && echo "OK: SVG parses"`
Expected: `OK: SVG parses`. If `xmllint` is not installed (`command not found`), run `python3 -c "import xml.etree.ElementTree as ET; ET.parse('assets/banner.svg'); print('OK: SVG parses')"` instead.

- [ ] **Step 4: Verify the file is reasonably sized (sanity check — not a placeholder)**

Run: `wc -c assets/banner.svg`
Expected: between 1500 and 3000 bytes (hand-authored, not a stub)

- [ ] **Step 5: Commit**

```bash
git add assets/banner.svg
git commit -m "feat(assets): add custom SVG profile banner"
```

---

## Task 3: Add placeholder SVGs so the README renders before the workflow's first run

**Files:**
- Create: `assets/stats.svg`
- Create: `assets/streak.svg`

The README references these two files. They need to exist in the repo immediately or the README will show broken-image icons until the workflow runs (24h max). Ship a small placeholder for each, then let the workflow overwrite them on its first run.

- [ ] **Step 1: Write `assets/stats.svg`**

Create `assets/stats.svg` with the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 495 195" width="495" height="195" role="img" aria-label="GitHub stats placeholder — regenerated by Actions within 24h">
  <rect width="495" height="195" fill="#0d1117" />
  <text x="247" y="100" text-anchor="middle" fill="#8b949e" font-family="Menlo, Consolas, monospace" font-size="18">stats — regenerating…</text>
</svg>
```

- [ ] **Step 2: Write `assets/streak.svg`**

Create `assets/streak.svg` with the following content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 495 195" width="495" height="195" role="img" aria-label="GitHub streak placeholder — regenerated by Actions within 24h">
  <rect width="495" height="195" fill="#0d1117" />
  <text x="247" y="100" text-anchor="middle" fill="#8b949e" font-family="Menlo, Consolas, monospace" font-size="18">streak — regenerating…</text>
</svg>
```

- [ ] **Step 3: Verify both files exist and parse**

Run:
```bash
for f in assets/stats.svg assets/streak.svg; do
  python3 -c "import xml.etree.ElementTree as ET; ET.parse('$f'); print('OK: $f')"
done
```
Expected:
```
OK: assets/stats.svg
OK: assets/streak.svg
```

- [ ] **Step 4: Commit**

```bash
git add assets/stats.svg assets/streak.svg
git commit -m "feat(assets): add placeholder stats/streak SVGs"
```

---

## Task 4: Add the GitHub Actions workflow

**Files:**
- Create: `.github/workflows/stats.yml`

This workflow regenerates the two SVGs daily and on manual trigger, then commits them back to the repo.

- [ ] **Step 1: Ensure the workflow directory exists**

Run: `mkdir -p .github/workflows`

- [ ] **Step 2: Write the workflow file**

Create `.github/workflows/stats.yml` with the following content (exact bytes):

```yaml
name: Regenerate profile stats

on:
  schedule:
    - cron: "0 6 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  stats:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Generate stats card
        uses: stats-organization/github-readme-stats-action@v2
        with:
          card: stats
          options: username=medsabbar&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true
          path: assets/stats.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Generate streak card
        uses: DenverCoder1/github-readme-streak-stats@v1
        with:
          options: theme=tokyonight
          path: assets/streak.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Commit regenerated SVGs
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add assets/stats.svg assets/streak.svg
          if git diff --cached --quiet; then
            echo "No changes to commit."
            exit 0
          fi
          git commit -m "chore(stats): regenerate stats and streak SVGs [skip ci]"
          git push
```

- [ ] **Step 3: Validate the workflow YAML**

Run: `python3 -c "import yaml; yaml.safe_load(open('.github/workflows/stats.yml')); print('OK: workflow YAML parses')"`
Expected: `OK: workflow YAML parses`

- [ ] **Step 4: Verify the workflow references no third-party endpoints**

Run: `grep -nE "herokuapp|vercel.app|cyclic.sh" .github/workflows/stats.yml || echo "OK: no third-party endpoints"`
Expected: `OK: no third-party endpoints`

- [ ] **Step 5: Commit**

```bash
git add .github/workflows/stats.yml
git commit -m "ci(workflows): regenerate stats SVGs daily via Actions"
```

---

## Task 5: Set up `markdownlint` for verification

**Files:**
- Create: `.markdownlint.json` (light config, GitHub-flavored Markdown friendly)
- Modify: `package.json` (add markdownlint as a devDependency + a `lint` script)

The repo has no `package.json` yet, so we are adding minimal Node-based tooling. This is the only non-content change in the plan and it is the verification harness the spec requires.

- [ ] **Step 1: Initialize a minimal `package.json`**

Run:
```bash
cat > package.json <<'EOF'
{
  "name": "medsabbar-profile",
  "private": true,
  "scripts": {
    "lint": "markdownlint README.md"
  },
  "devDependencies": {
    "markdownlint-cli": "^0.42.0"
  }
}
EOF
```

- [ ] **Step 2: Write a permissive `.markdownlint.json`**

Create `.markdownlint.json` with the following content:

```json
{
  "default": true,
  "MD013": false,
  "MD033": false,
  "MD041": false,
  "MD024": { "siblings_only": true }
}
```

Notes on the overrides:
- `MD013` off — line length is fine to exceed on prose lines.
- `MD033` off — we use HTML in the README (the centering div).
- `MD041` off — first-line-doesn't-need-to-be-h1 is fine for a profile README.
- `MD024` siblings-only — multiple `##` sections of the same level is allowed if they are not siblings.

- [ ] **Step 3: Install the dev dependency**

Run: `npm install`
Expected: completes with no errors. `node_modules/` and `package-lock.json` are created.

- [ ] **Step 4: Run the linter**

Run: `npm run lint`
Expected: exits 0. The lint command targets `README.md` only — internal spec/plan documents in `docs/superpowers/` are out of scope for this lint and are not verified. If the linter reports issues, fix them in `README.md` and re-run until it passes. The most likely issue is a stray `---` setext-style underline — convert to `###` if so.

- [ ] **Step 5: Add `node_modules/` to `.gitignore`**

Edit `.gitignore` to append `node_modules/` (the file currently contains only `.worktrees/`). Final content:

```
.worktrees/
node_modules/
```

- [ ] **Step 6: Commit the tooling**

```bash
git add package.json package-lock.json .markdownlint.json .gitignore
git commit -m "chore(tooling): add markdownlint for README verification"
```

---

## Task 6: Final verification pass

**Files:**
- Read-only checks against the final tree.

- [ ] **Step 1: Confirm all expected files are present**

Run:
```bash
ls -1 README.md assets/banner.svg assets/stats.svg assets/streak.svg .github/workflows/stats.yml package.json .markdownlint.json
```
Expected: all 7 paths listed, no `No such file or directory`.

- [ ] **Step 2: Confirm no third-party hosts anywhere in tracked files**

Run:
```bash
git grep -nE "herokuapp|vercel\.app|cyclic\.sh" || echo "OK: no third-party hosts in tracked files"
git grep -nE 'http://' -- ':!assets/banner.svg' || echo "OK: no http:// fetches outside SVG xmlns"
```
Expected:
```
OK: no third-party hosts in tracked files
OK: no http:// fetches outside SVG xmlns
```
(The `xmlns="http://www.w3.org/2000/svg"` namespace URI in the banner is excluded from the second check — it is a URI identifier, not a network fetch.)

- [ ] **Step 3: Confirm no pinned-projects block**

Run:
```bash
git grep -nE "pinned|Pinned|PINNED|tab=repositories" README.md || echo "OK: no pinned block in README"
```
Expected: `OK: no pinned block in README`.

- [ ] **Step 4: Confirm line count is within budget**

Run: `wc -l README.md`
Expected: `<= 45`.

- [ ] **Step 5: Run the linter one more time**

Run: `npm run lint`
Expected: exits 0.

- [ ] **Step 6: Visually inspect the README**

Open `README.md` in a Markdown previewer (VS Code, GitHub Desktop, or `grip`/`mdcat` if available) and confirm:
- Banner image references `./assets/banner.svg` and has descriptive alt text
- Hero line reads: **CTO at IBTIKAR-Technologies. Building the web, one less-buggy app at a time.**
- Stack line lists exactly: `Node.js` · `Next.js` · `MongoDB` · `Firebase` · `TypeScript`
- Both stats SVGs are referenced with alt text

- [ ] **Step 7: Push the branch**

```bash
git push -u origin feature/profile-revamp
```

- [ ] **Step 8: Open a PR (manual, on github.com)**

Go to https://github.com/medsabbar/medsabbar/compare/main...feature/profile-revamp and open a pull request titled:

```
Revamp GitHub profile README
```

PR body should include:
- One-line summary: "Gut the cliché profile README, ship a custom banner and self-hosted stats."
- The acceptance-criteria checklist from the design spec §8.3, each item checked.
- A note that the workflow will run on merge (or can be triggered manually from the Actions tab) to replace the placeholder SVGs with real data.

---

## Self-Review Notes

- **Spec coverage:** Spec §4 (architecture) → Tasks 1–4. §5 (README content) → Task 1. §6 (banner spec) → Task 2. §7 (workflow) → Task 4. §8.2 structural checks → Task 6. §8.3 acceptance criteria → Task 6 + Task 5 (markdownlint). All sections mapped.
- **Placeholder scan:** No "TBD"/"TODO"/"implement later" anywhere. Every step has exact code or exact commands. The only `cat > package.json <<'EOF' ... EOF` heredoc in Task 5 is a one-shot, not a placeholder.
- **Type/name consistency:** SVG class names (`bg`, `rule`, `name`, `handle`, `tagline`, `footer`) are consistent between Task 2's definition and the README's alt text. Workflow step names are consistent between Task 4's `steps:` list and the commit messages.
- **Third-party hosts:** Re-checked. The only `http://` reference in tracked files is the SVG `xmlns` namespace, which is a URI identifier, not a fetch — allowed and necessary for SVG parsing.
- **Pinned projects block:** Explicitly checked in Tasks 1, 4 (none there), and Task 6 step 3.
