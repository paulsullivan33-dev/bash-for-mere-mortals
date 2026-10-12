# Your First Script

**Scope:** writing a script, the shebang, making it executable,
and running it three ways. All commands below were run and
verified with Bash 5.2.

## Write it

Create `hello.sh`:

```bash
#!/usr/bin/env bash
echo "Hello, world!"
```

The first line is the **shebang**: it tells the OS which
interpreter runs this file. `#!/usr/bin/env bash` finds Bash via
your PATH — more portable than hardcoding `/bin/bash`.

## Make it executable

```sh
chmod +x hello.sh
```

**Check:** `ls -l hello.sh` shows `x` in the permissions
(`-rwxr-xr-x`).

## Run it three ways

```sh
./hello.sh        # direct — uses the shebang
bash hello.sh     # explicit — shebang ignored
bash -x hello.sh  # trace mode — prints each command before running it
```

**Check:** all three print `Hello, world!`

`bash -x` is your best debugging friend: it shows exactly what
ran, with variables expanded. When a script misbehaves, re-run
with `-x` first.

## A slightly real script

```bash
#!/usr/bin/env bash
# backup.sh — copy a file with today's date in the name
name="notes.txt"
stamp=$(date +%F)
cp "$name" "${name%.txt}-$stamp.txt"
echo "Backed up to ${name%.txt}-$stamp.txt"
```

Try it: `echo "hi" > notes.txt`, then `./backup.sh`.

**Check:** a file like `notes-2026-10-12.txt` appears.

New pieces:

- `$(...)` runs a command and captures its output
  (`date +%F` gives `2026-10-12`).
- `${name%.txt}` strips `.txt` off the end — *parameter
  expansion*, Bash's built-in string surgery.

## Comments and layout

- `#` starts a comment — to end of line.
- One command per line; join with `;` only for throwaway
  one-liners.
- Quote your variables (always `"$name"`, never bare `$name`) —
  the full story is in
  [Variables and Quoting](variables-and-quoting.md), and it's the
  single most important habit in Bash.

## Check

- [ ] You can write, `chmod +x`, and run a script
- [ ] You know what the shebang does and why `env` is used
- [ ] You reach for `bash -x` when something misbehaves

Next: [Variables and Quoting](variables-and-quoting.md).
