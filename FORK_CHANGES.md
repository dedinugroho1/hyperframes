# Fork Changes

This file records every change this fork makes to files from the upstream
project, as required by **Section 4(b) of the Apache License 2.0**:

> You must cause any modified files to carry prominent notices stating that You changed the files.

|                 |                                                                                    |
| --------------- | ---------------------------------------------------------------------------------- |
| **Fork**        | [dedinugroho1/hyperframes](https://github.com/dedinugroho1/hyperframes)            |
| **Maintainer**  | Dedi Nugroho                                                                       |
| **Upstream**    | [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) (HeyGen, Inc.) |
| **Forked from** | upstream `main` at commit `2905162d9`                                              |
| **License**     | Apache License 2.0, unchanged (see [`LICENSE`](LICENSE))                           |

Any file **not** listed below is identical to upstream at the commit it was
last synced from.

## Change log

Newest entries first. One row per file per change.

| Date       | File                                 | Type     | Description                                                                                                                                                                                                                     |
| ---------- | ------------------------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-10-12 | [`README.md`](README.md)             | Modified | Added the fork header (badges, unofficial-fork notice, About This Fork, from-source Quick Start) above the upstream content; replaced the `License` section with `Credits & License`. Upstream content in between is unchanged. |
| 2026-10-12 | [`NOTICE`](NOTICE)                   | Added    | Attribution notice for HeyGen, Inc. and the fork; trademark disclaimer.                                                                                                                                                         |
| 2026-10-12 | [`FORK_CHANGES.md`](FORK_CHANGES.md) | Added    | This change log.                                                                                                                                                                                                                |

Type is one of `Added`, `Modified`, `Removed`, or `Renamed`.

## How to record a change

Every time you change a file that came from upstream, do **both** of the
following in the same commit.

### 1. Add a row to the change log above

State the date, the file, and what changed, plainly enough that a reader can
tell the fork's code from HeyGen's.

### 2. Put a notice in the modified file itself

Add a short comment near the top of the file, **below** any existing
copyright or license header (never edit or remove HeyGen's notices):

```ts
// Modified by Dedi Nugroho, 2026-10-12: <one-line summary of the change>.
// See FORK_CHANGES.md. Original: https://github.com/heygen-com/hyperframes
```

Use the comment syntax of the file type:

| File type                     | Notice format                                              |
| ----------------------------- | ---------------------------------------------------------- |
| `.ts` `.tsx` `.js` `.mjs`     | `// Modified by Dedi Nugroho, YYYY-MM-DD: ...`             |
| `.css`                        | `/* Modified by Dedi Nugroho, YYYY-MM-DD: ... */`          |
| `.html` `.md` `.mdx` `.svg`   | `<!-- Modified by Dedi Nugroho, YYYY-MM-DD: ... -->`       |
| `.sh` `.yml` `.yaml` `.toml`  | `# Modified by Dedi Nugroho, YYYY-MM-DD: ...`              |
| `.json` (no comments allowed) | No in-file notice; the change log row above is the notice. |

For a file changed several times, keep one notice in the file and update its
date and summary; the change log keeps the full history.

### Brand-new files

Files that do not exist upstream need no "modified" notice. Optionally start
them with:

```ts
// Copyright 2026 Dedi Nugroho. Licensed under the Apache License, Version 2.0.
```

## Rules this fork follows

- **`LICENSE` is never edited.** It stays the unmodified Apache 2.0 text with
  HeyGen, Inc.'s copyright line.
- **Upstream copyright and attribution notices are never removed** from any
  file (Section 4(c)).
- **`NOTICE` is carried with every redistribution** of this fork, including
  packaged or binary builds (Section 4(d)).
- **No claim of being official** (Section 6). This fork does not present
  itself as a HeyGen release and does not use HeyGen's names or logos
  beyond describing where the code comes from. If you publish packages from
  this fork, use a different package name or scope than the official
  `hyperframes` / `@hyperframes/*` packages.

## Syncing with upstream

```bash
git remote add upstream https://github.com/heygen-com/hyperframes.git   # once
git fetch upstream
git merge upstream/main
```

After a sync, update the **Forked from** commit at the top of this file and
check that each file in the change log still carries its notice.
