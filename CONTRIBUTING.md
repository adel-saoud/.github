# Contributing

This guide applies to every repository under `adel-saoud`. Project-specific guides
(how to add a provider, a metric, a storage migration) are in each repository's
`docs/` folder, usually `docs/extending.md`.

## Setup

You need [uv](https://docs.astral.sh/uv/). It installs the Python version pinned in
`.python-version` on first use.

```bash
uv sync --all-extras
uv run pre-commit install
uv run pre-commit install --hook-type commit-msg
```

## Quality gate

Run both before opening a pull request. CI runs the same two commands.

```bash
uv run pre-commit run --all-files
uv run pytest
```

- `pre-commit` runs ruff (lint and format), pyright, gitleaks and the standard file checks.
- `pytest` fails below 90% coverage.
- Tests are hermetic: no network calls and no API keys. Live LLM evals run in a separate,
  manually triggered workflow.

## Commits

[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/): `feat:`, `fix:`,
`docs:`, `refactor:`, `test:`, `chore:`. Add `!` after the type for a breaking change.
commitizen checks the message in the `commit-msg` hook. Releases use `cz bump`, which
writes the version and the tag. `CHANGELOG.md` is written by hand: describe user-facing
changes there in the same PR.

## Decisions

Design choices are recorded in `docs/DECISIONS.md`, one `### Why X instead of Y?`
question per choice: what was chosen, what was rejected, the trade-off, and what would
change the answer. If your change reverses or adds a choice, update that file in the
same pull request. Larger choices get a short ADR in `docs/adr/`.

## Pull requests

- One topic per pull request, with a test for every behaviour change.
- Add a line under `## [Unreleased]` in `CHANGELOG.md`.
- Opening an issue first is welcome for anything larger than a fix, so we agree on the
  approach before you write it.

Security issues: please do not open a public issue. See the
[security policy](https://github.com/adel-saoud/.github/blob/main/SECURITY.md).
