# mend

[![make.fun bounty](https://img.shields.io/badge/make.fun-bounty-7c3aed)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122)
[![prize $100](https://img.shields.io/badge/prize-%24100%20top%2010-16a34a)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122)
[![spec](https://img.shields.io/badge/spec-SPEC.md-blue)](https://make.fun/bounty/hidden-spec-cli-01-run-20261122)

`mend` applies an edit that an AI model wrote to a tree of text files. Models write edits in
four formats — a unified diff (often with wrong line numbers), SEARCH/REPLACE blocks, the V4A
patch format, and text-editor tool calls as JSON — and `mend` reads each one as `SPEC.md` says,
finds the lines each edit names at the first of three matching tiers, and changes every file or
none at all.

This repository is an entry for the [make.fun bounty](https://make.fun/bounty/hidden-spec-cli-01-run-20261122)
"Build mend, one patch tool for every AI edit format" ($100, top 10 × $10).

## Run

Node 22 and its standard library alone — nothing to install:

```sh
node main.ts [--format=NAME] [--check] [--strict] [PATCH]
```

- The working directory is the root of the tree. Every path in a patch is relative to it.
- `PATCH` is the path of the patch file, relative to the working directory. When `PATCH` is
  absent or is `-`, the patch is read from standard input.
- `--format=NAME` reads the patch in one format: `unified`, `search-replace`, `v4a` or `json`;
  `auto`, the default, detects it.
- `--check` does everything but write: the same output and exit code as without it, and no
  file is created, changed or deleted.
- `--strict` matches lines exactly (tier 0 only).

Exit `0` prints one line per changed path (`A` added, `D` deleted, `M` modified,
`R old -> new` moved) and `ok: <n> files`. Exit `1` prints one `error: <code> <path>` line
per failed edit and `failed: <k> of <n> edits` — a failed edit changes nothing, and the
edits after it still run, so no file is written at all. Exit `2` prints one line for a
usage error, an unreadable patch file, an undetected format, or a parse error
(`error: parse <N>`).

## Files

- `main.ts` — the whole tool.
- `runner.ts` — the kit's trial runner, kept as it is; `main.ts` hands a trial over to it
  when `MAKEFUN_ENV_URL` is set.
- `SPEC.md` (in the kit, not this repo) is the whole contract.

## Practice score

100 of 100 practice checks pass (practice score 1000): unified 30/30, SEARCH/REPLACE 20/20,
V4A 20/20, JSON 15/15, command line 15/15, verified with the kit's `check.ts`.
