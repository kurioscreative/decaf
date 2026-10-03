# decaf

Kill the `caffeinate` sessions you started from your terminal, in one command.

If you run `caffeinate` (with any flags: `-i`, `-d`, `-t 3600`, ...) from different terminals and forget about it, the sessions pile up and keep your Mac awake. `decaf` lists them, then kills them all.

```console
$ decaf
36014 caffeinate -i
56112 caffeinate -d -t 3600
Killed 2 caffeinate process(es).
Left 2 started by other apps running (use --all to kill those too).
```

Apps also use `caffeinate` to keep your Mac awake while they work (Claude Code does, for example). `decaf` leaves those alone by default. Pass `--all` to kill every `caffeinate` you own.

## Install

```bash
brew install kurioscreative/tap/decaf
```

Update with `brew upgrade decaf`.

### Without Homebrew

```bash
mkdir -p ~/.local/bin
curl -fsSL https://raw.githubusercontent.com/kurioscreative/decaf/main/decaf -o ~/.local/bin/decaf
chmod +x ~/.local/bin/decaf
```

Make sure `~/.local/bin` is on your `PATH`. If it isn't, add this line to `~/.zshenv`:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

## How it works

1. `pgrep -x -u "$USER" caffeinate` finds your caffeinate processes. `-x` matches the exact process name, and `-u` limits it to your own processes.
2. For each one, `decaf` looks at its parent process. It kills the session if the parent is a shell (`zsh`, `bash`, `fish`, ...) or if the parent is gone (an orphan, reparented to `launchd`, so nothing relies on it). Anything else was started by an app and is left alone unless you pass `--all`.

If you started a command under caffeinate (`caffeinate -i make build`), `decaf` stops the caffeinate wrapper but not the command itself. The command keeps running; your Mac just stops being kept awake.

## License

MIT
