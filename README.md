# decaf

Kill every `caffeinate` process you started on macOS, in one command.

If you run `caffeinate -i` from different terminals and forget about it, the sessions pile up and keep your Mac awake. `decaf` lists them, then kills them all.

```console
$ decaf
36014 caffeinate -i
56112 caffeinate -i
Killed 2 caffeinate process(es).
```

## Install

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

```bash
pkill -x -u "$USER" caffeinate
```

- `-x` matches the exact process name `caffeinate`, so it won't kill a process that merely mentions "caffeinate" in its arguments.
- `-u "$USER"` only touches your own processes, never ones owned by the system or other users.

If you started a command under caffeinate (`caffeinate -i make build`), `decaf` stops the caffeinate wrapper but not the command itself. The command keeps running; your Mac just stops being kept awake.

## License

MIT
