<p align="center">
  <img src="assets/gray-logo.svg" alt="gray" width="96">
</p>
<h1 align="center">gray-autocommit</h1>
<p align="center">Commit uncommitted changes when the turn ends (opt-in).</p>
<p align="center">
  <a href="https://github.com/vstaln/gray-autocommit/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
  <img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-7aa2f7.svg">
  <img alt="rust" src="https://img.shields.io/badge/built%20with-rust-orange.svg">
</p>

Commit uncommitted changes when the turn ends (opt-in).

On `turn_end`, when enabled and the session cwd is a git work tree with a
non-empty `git status --porcelain`: `git add -A` + `git commit -m "gray:
auto-commit (session <id>)"`. Never commits outside a repo, never on a
clean tree, never on a turn where no tool ran. Default **OFF**.

A sidecar plugin for [gray](https://github.com/vstaln/gray), scaffolded by
[gray-account](https://github.com/vstaln/gray-account).

## Wire methods used

- `plugin/manifest`, `plugin/shutdown`
- `event/notify` — `pre_tool`/`post_tool` mark the turn as active,
  `turn_end` runs the commit check (`git -C <session.cwd>`, falls back to
  the process cwd)
- `command/run` — `/autocommit on|off|status` (persisted at
  `~/.gray/autocommit/state`)

No capabilities required.

## Install

```sh
gray plugin install autocommit
gray plugin run /autocommit on    # enable (opt-in)
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```

Bump `version` in `Cargo.toml` before each `publish`; the registry refuses to
republish a version.

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
