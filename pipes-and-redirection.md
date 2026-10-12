# Pipes and Redirection

**Scope:** stdin/stdout/stderr, redirecting to files, and piping
commands together. All examples were run with Bash 5.2.

## The three streams

Every command has three streams:

| Stream | Number | Default |
|---|---|---|
| stdin | 0 | Your keyboard |
| stdout | 1 | Your terminal |
| stderr | 2 | Your terminal |

Normal output goes to stdout; errors and diagnostics go to
stderr. Keeping them separate is what makes the rest of this
guide possible.

## Redirecting to files

```bash
echo "hello" > out.txt    # stdout → file (overwrites)
echo "again" >> out.txt   # stdout → file (appends)
./script.sh 2> err.log    # stderr → file
./script.sh > all.log 2>&1  # both → file
```

- `>` overwrites; `>>` appends. Mixing them up deletes data —
  double-check before `>`.
- `2>&1` means "send stderr where stdout is going." Order
  matters: `> file 2>&1` works; `2>&1 > file` doesn't do what
  you want.

## Piping: stdout becomes stdin

```bash
cat access.log | grep " 404 " | wc -l
```

The pipe `|` connects commands: each command's stdout feeds the
next command's stdin. Read pipelines right-to-left to understand
them: "count lines, of 404s, in the log."

Long pipelines get unreadable — break them or save intermediates
when debugging.

## /dev/null and heredocs

```bash
./noisy.sh > /dev/null 2>&1   # silence everything
./noisy.sh > /dev/null        # silence stdout, keep errors
```

`/dev/null` discards whatever you send it.

A **heredoc** feeds multi-line input to a command:

```bash
cat << 'EOF' > config.ini
[server]
port = 8080
host = localhost
EOF
```

Quoted `'EOF'` means no expansion inside; unquoted `EOF` would
expand `$variables`. Prefer quoted unless you want expansion.

## Process substitution: files from commands

```bash
diff <(ls dir1) <(ls dir2)
```

`<(...)` pretends a command's output is a file — for commands
like `diff` that demand filename arguments, not stdin.

## tee: see it and save it

```bash
./build.sh 2>&1 | tee build.log
```

`tee` copies stdin to both stdout and a file — watch the output
live *and* keep the log.

## xargs: arguments from stdin

```bash
find . -name "*.tmp" | xargs rm
```

`xargs` turns stdin lines into command arguments. The safe form
for weird filenames:

```bash
find . -name "*.tmp" -print0 | xargs -0 rm
```

(`-print0`/`-0` use null separators instead of whitespace.)

## Exit codes and PIPESTATUS

```bash
false | true
echo $?          # 0 — exit code of the LAST command only!
echo "${PIPESTATUS[0]}"  # 1 — the real story
```

`$?` after a pipeline is just the last command's code. With
`set -o pipefail` (see [Error Handling](error-handling.md)), the
pipeline fails if *any* stage fails.

## Check

- [ ] You can redirect stdout, stderr, and both — append and
      overwrite
- [ ] You can read a pipeline right-to-left
- [ ] You know why `$?` lies after a pipeline and the fix

Next: [Text Processing](text-processing.md).
