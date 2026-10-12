# Troubleshooting

**Scope:** the errors and gotchas you'll actually hit, what they
mean, and the plain fix for each. Verified against Bash 5.2.

## Errors

### "command not found: name"

You typed `name = "Ada"` with spaces — Bash read `name` as a
command. **Fix:** remove the spaces: `name="Ada"`.

### "No such file or directory" on a script that exists

Two usual causes:

1. **Missing shebang or `chmod +x`** when running `./script.sh`.
2. **Windows line endings** (`\r\n`) — the `\r` becomes part of
   the interpreter path or arguments. **Fix:** `dos2unix
   script.sh` or `sed -i 's/\r$//' script.sh`.

### "unbound variable"

`set -u` caught you using a variable that was never set — often
a typo. **Fix:** check the spelling; use `${var:-default}` if
empty is legitimate.

### "syntax error near unexpected token"

Usually a paren/brace/quote mismatch, or Windows line endings.
**Fix:** run with `bash -n script.sh` (syntax check only) to find
the line, then eyeball the quoting.

### "ambiguous redirect"

```bash
echo hi > $outfile   # fails if $outfile is empty or has spaces
```

**Fix:** quote it: `> "$outfile"`, and make sure it's set
(`set -u` catches the empty case).

## Gotchas (runs fine, surprises you)

### Unquoted variables word-split

```bash
file="my notes.txt"
rm $file   # tries to remove "my" and "notes.txt"!
```

**Fix:** `"$file"`, always. (See
[Variables and Quoting](variables-and-quoting.md).)

### for line in $(cat file)

Word-splits and glob-expands the file contents. **Fix:** the
`while IFS= read -r line` loop.

### Glob didn't match anything

`for f in *.txt` with no `.txt` files iterates once with the
literal string `*.txt`. **Fix:** `shopt -s nullglob`.

### cd failed, rest of script ran elsewhere

```bash
cd "$dir"
rm -rf ./*   # if cd failed, this runs in the WRONG directory
```

**Fix:** `cd "$dir" || exit 1` — always guard `cd`.

### Variables set inside a pipeline disappear

```bash
cat f.txt | while read -r l; do count=$((count+1)); done
echo "$count"  # empty — the loop ran in a subshell
```

Each pipeline stage runs in a subshell; its variables die with
it. **Fixes:** redirect instead of piping (`while read... done
< f.txt`), or `shopt -s lastpipe` (Bash 4.2+, non-interactive
only).

### set -e didn't stop on failure

It's ignored in `if` conditions, `&&`/`||` lists, and command
substitutions in some positions. If you need guaranteed failure,
be explicit: `cmd || die "cmd failed"`.

## Debugging technique

1. `bash -n script.sh` — syntax check, runs nothing.
2. `bash -x script.sh` — trace every command as it runs.
3. Narrow it: `set -x` / `set +x` around the suspicious section.
4. `shellcheck script.sh` — a linter that catches most of the
   above automatically. Install it; run it on every script.

**ShellCheck** deserves emphasis: it's like a compiler warning
system for Bash, and it will teach you the language faster than
any guide. `apt install shellcheck` / `brew install shellcheck`.

## Check

- [ ] You check `bash -n` then `bash -x` when debugging
- [ ] You've installed ShellCheck and run it on a script
- [ ] You guard every `cd` with `|| exit 1`

Next: [Glossary](glossary.md).
