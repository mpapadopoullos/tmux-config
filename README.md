# tmux config

Personal [tmux](https://github.com/tmux/tmux) configuration for **macOS +
[Ghostty](https://github.com/ghostty-org/ghostty)**. Tested on tmux 3.8;
requires tmux ≥ 3.2 (`extended-keys`, `display-popup`, `copy-command`).

## Install

```sh
git clone https://github.com/mpapadopoullos/tmux-config ~/.config/tmux
~/.config/tmux/plugins/tpm/bin/install_plugins   # or: prefix I
```

Requirements:

- [tpm](https://github.com/tmux-plugins/tpm) plugins (installed above)
- [fzf](https://github.com/junegunn/fzf) — optional, for the `prefix s` / `prefix w` pickers
- `pbcopy` — macOS built-in

The prefix is `C-a`.

## Key bindings

| Keys | Action |
|---|---|
| `prefix r` | Reload configuration |
| `prefix c` | New window (in current path) |
| `prefix \|` / `prefix _` | Split horizontal / vertical (in current path) |
| `prefix q` / `prefix Q` | Kill pane / window (no confirmation) |
| `prefix z` | Zoom pane |
| `prefix h j k l` | Move between panes (vim-style) |
| `option ←` / `option →` (no prefix) | Previous / next window |
| `prefix Tab` | Toggle between the two most recent windows |
| `prefix <` / `prefix >` | Swap window left / right |
| `prefix m` then `1`–`9` | Move window to position — browser-style: windows from that position on shift +1; beyond the last window appends at the end (any other key cancels) |
| `prefix !` | Break pane into its own window |
| `prefix J` | Join pane from another window |
| `prefix @` | Send current pane into another window |
| `prefix S` | Toggle synchronize-panes (type into all panes at once) |
| `prefix t` | Floating scratch shell in the current directory (`exit` closes it) |
| `prefix s` / `prefix w` | fzf session / window picker |
| `y` / mouse drag (copy mode) | Copy to system clipboard (tmux-yank) |

Note on window switching: with Ghostty's default (`macos-option-as-alt` off),
option+←/→ arrive as `ESC b` / `ESC f`, so those keys switch windows instead of
moving by words in the shell.

## Notable settings

- `tmux-256color` inner terminal with an explicit `xterm-ghostty:RGB` override
  (true color through Ghostty)
- vi mode for both the status line and copy mode
- 50k scrollback history, mouse support, 2 s message display time
- Windows numbered from 1, renumbered on close; `kill-session` hops to the
  next session instead of detaching
- Sessions auto-restored on tmux server start (continuum + resurrect, with
  pane contents)

## Plugins

Managed by [tpm](https://github.com/tmux-plugins/tpm):
[resurrect](https://github.com/tmux-plugins/tmux-resurrect),
[continuum](https://github.com/tmux-plugins/tmux-continuum),
[yank](https://github.com/tmux-plugins/tmux-yank).
