# vps-picker

An fzf dropdown on your laptop for tmux sessions living on a remote box,
attached over [mosh](https://mosh.org) so the connection survives sleep,
network changes, and flaky wifi. Built for the workflow of running
[Claude Code](https://docs.anthropic.com/en/docs/claude-code) inside
persistent tmux sessions on a VPS.

```
features10  1w  attached  ● claude  ✳ Session history and topics feature
features20  1w  attached  ● claude  ✳ blob-storage refactor
qa10        1w  detached  ● claude  ✳ Leave evidence and screenshots
scratch     1w  detached  -
```

- **enter** on a row — attach to that session (mosh + `tmux new -As`)
- **type a name + ctrl-n** — create and attach a new session (enter also works
  when the name matches nothing)
- **ctrl-x** — kill the selected session
- **ctrl-r** — refresh the list
- `● claude` — a Claude Code process is running in that session
- last column — the pane title (Claude Code sets it to its current task)

## Install

Requirements: `fzf` and `mosh` locally; `tmux` and `mosh` on the server.

```bash
install -m755 vps ~/.local/bin/vps
```

Define your host in `~/.ssh/config`:

```
Host vps
  HostName <your-server>
  User <your-user>
  ServerAliveInterval 30
```

Server side, allow mosh's UDP range if firewalled:

```bash
sudo apt-get install -y mosh
sudo ufw allow 60000:61000/udp
```

## Usage

```bash
vps            # picker for the default host ($VPS_DEFAULT_HOST or "vps")
vps otherhost  # picker for any ssh config alias
```

## Why

TCP ssh dies when the laptop sleeps. tmux keeps the work alive server-side;
mosh keeps the *view* alive client-side. This script removes the
`ssh → tmux ls → tmux attach` ritual in between.
