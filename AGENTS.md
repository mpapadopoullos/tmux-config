# AGENTS.md

Guidance for AI agents working in this repository.

## What this is

Personal tmux dotfiles. `tmux.conf` is the single source of truth; `tmux.theme`
is sourced at the end if present. Target environment: **macOS, Ghostty, tmux
3.8** (check `tmux -V`, outer `TERM=xterm-ghostty`). fzf is installed.

## Making changes

- Edit `tmux.conf`, then reload and verify — do not claim a change works
  without exercising it: `tmux source-file ~/.config/tmux/tmux.conf`.
- Never test against the user's live session or with untargeted commands
  (`move-window` without `-s`/`-t` acts on the "current" window, which from
  the CLI may be the user's session). Use throwaway sessions:

  ```sh
  tmux new-session -d -s cfgtest -x 80 -y 24 -n w1
  # ... test ...
  tmux kill-session -t cfgtest
  ```

- For end-to-end key-binding tests (key tables need a real client), attach a
  scratch session through a pty and feed keystrokes:

  ```sh
  (printf '\001m'; sleep 0.4; printf '2'; sleep 1; printf '\001d') \
    | script -q /dev/null tmux attach -t cfgtest
  ```

## tmux gotchas discovered here (do not relearn these)

- tmux 3.8 ships a **native `move` key table** (floating-pane placement,
  digits 1–4 bound). Custom tables must not be named `move` — this config
  uses `mwin`.
- `move-window -b` (insert-before) produces garbage on **gapped window
  indices**. Always `move-window -r` before inserting, and again after to
  close the shift gap. The `mwin` bindings follow this pattern.
- A failing command in a `\;` sequence **aborts the remaining commands**.
  Order sequences so state is restored first (e.g. `switch-client -T root`
  before the fallible command).
- `if-shell` branch command strings **cannot contain `\;` sequences** — the
  escaped semicolon is parsed as a junk argument ("too many arguments").
- `move-window` without `-s` moves the **current window** (binding context).
- `terminal-overrides` patterns match the *outer* terminal (`xterm-ghostty`),
  not `default-terminal`.

## Git conventions

- Signed conventional commits: `git commit -S -s` (repo-local identity and
  SSH signing key are configured in `.git/config`; do not touch global git
  config).
- Amend/rewrite requires explicit user request. When rewriting history,
  push with `git push --force-with-lease` only.
- Never push without user approval.
- The initial import tracks `plugins/*` gitlinks and `*.bak` files; leave
  them unless the user asks for a cleanup.
