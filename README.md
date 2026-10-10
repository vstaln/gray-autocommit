<div align="center">
  <img alt="gray-autocommit" src="assets/icon.svg" width="120" height="120" />
  <h1>gray-autocommit</h1>
  <p><strong>Commit uncommitted changes when a turn ends (opt-in, off by default).</strong></p>
  <p>
    <a href="https://gray.alignment.id">Website</a> ·
    <a href="https://gray.alignment.id/plugins/gray-autocommit">Store</a> ·
    <a href="https://github.com/vstaln/gray-autocommit">Source</a> ·
    <a href="https://github.com/vstaln/gray">gray</a>
  </p>
  <p>
    <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-1c1c20?style=flat-square&labelColor=0a0a0b" /></a>
    <a href="https://www.rust-lang.org"><img alt="Built with Rust" src="https://img.shields.io/badge/built%20with-rust-1c1c20?style=flat-square&labelColor=0a0a0b&logo=rust&logoColor=d4a373" /></a>
    <a href="https://gray.alignment.id/plugins/gray-autocommit"><img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-1c1c20?style=flat-square&labelColor=0a0a0b&color=7aa2f7" /></a>
  </p>
</div>

<br/>

```bash
gray plugin install gray-autocommit
```

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

## Tags

`gray` `plugin` `autocommit` `rust`

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
