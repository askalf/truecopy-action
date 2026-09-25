# Contributing to truecopy-action

Thanks for your interest in improving **truecopy-action** — the GitHub Action
for [truecopy](https://github.com/askalf/truecopy), the supply-chain gate for AI
agents. It gates every agent skill & MCP server change in CI: verify
`truecopy.lock` for drift & poisoning, or poison-scan a manifest / marketplace.
Part of [Own Your Stack](https://github.com/askalf).

## Ground rules

- Be respectful. This project follows our [Code of Conduct](CODE_OF_CONDUCT.md).
- Found a security issue? **Do not open a public issue** — email
  **support@askalf.org** to report it privately.

## Development setup

This repo is a **composite GitHub Action** — `action.yml` plus a thin runner
that invokes the `truecopy` CLI (pinned by version via `npx`). There is no build
step and no local test harness; the action is exercised end-to-end by the
`ci.yml` workflow, which runs it against the fixtures in `fixtures/`.

```bash
git clone https://github.com/askalf/truecopy-action.git
cd truecopy-action
```

To try the underlying behavior locally, run the CLI the action wraps:

```bash
npx -y github:askalf/truecopy add fixtures/clean-mcp.json --lock /tmp/truecopy.lock
npx -y github:askalf/truecopy verify --lock /tmp/truecopy.lock
```

## Making a change

1. Branch off `master`.
2. Keep the change focused — one concern per PR.
3. If you change `action.yml` inputs or the runner logic, extend `ci.yml` and
   the `fixtures/` so both the pass paths (clean lock / clean manifest) and the
   fail paths (drifted skill / poisoned manifest) still assert correctly.
4. Open a pull request against `master`.

## What CI requires

Every PR must pass these `ci` checks to merge — each drives the action against a
fixture:

- `verify — clean lock passes`
- `verify — drifted skill fails the build`
- `scan — clean manifest passes`
- `scan — poisoned manifest fails the build`

OpenSSF Scorecard also runs on the repo.

## Conventions

- GitHub Actions are **pinned to a commit SHA**, never a mutable tag, and the
  `truecopy` CLI is pinned to a version tag (e.g. `#v0.8.0`). New or updated
  steps must keep this.
- Consumers pin this action by its major tag, `askalf/truecopy-action@v1`.
- Commit messages: short imperative subject, with a wrapped body explaining the
  *why* when it isn't obvious.

## Releases

A release is a `## [X.Y.Z] - YYYY-MM-DD` section in `CHANGELOG.md`. When it lands
on `master`, `.github/workflows/release.yml` tags `vX.Y.Z`, moves the major tag
(`v1`) to it when it is the newest in that series, and publishes the GitHub
release with that section as its notes. To tag a specific earlier commit, run
the workflow by hand with its `sha` input.
