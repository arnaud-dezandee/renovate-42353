# Reproduction: renovatebot/renovate#42353

Semantic commit prefix stripped from PR titles since Renovate v43.104.0.

## Problem

With `semanticCommits: "auto"` (the default), Renovate scans recent commit
messages to decide whether to prefix PR titles with conventional commit types
(`chore(deps):`, `fix:`, etc.).

Since v43.104.0 (#42328 replaced `conventional-commits-detector` with a custom
regex scorer), repositories whose history contains a majority of GitHub
"Merge pull request …" commits now score **negatively**, causing semantic
commits to be **disabled** even though the repo does use conventional commits.

**Before (≤ 43.103.0):**
```
semanticCommits: detected "angular"
semanticCommits: enabled
```

**After (≥ 43.104.0):**
```
semanticCommits: score=-4
semanticCommits: disabled
```

## This repository

This repo has **3 conventional commits** and **8 merge commits** — a realistic
ratio for a small GitHub project that merges PRs via the merge button. The old
detector correctly identified the "angular" convention; the new scorer yields a
negative score and disables semantic commits.

## Workaround

```json
{
  "semanticCommits": "enabled"
}
```

## Files

- `renovate.json` — default Renovate config (`config:recommended`)
- `package.json` — one outdated dependency (`lodash@4.17.20`) so Renovate has
  something to update
