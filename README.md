# gray-autocommit

Commit uncommitted changes when the turn ends (opt-in). Port of pi's auto-commit-on-exit extension.

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
