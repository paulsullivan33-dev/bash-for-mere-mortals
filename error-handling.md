# Error Handling

**Scope:** exit codes, `set -euo pipefail`, and deliberate
failure. The difference between a script and a liability. All
examples were run with Bash 5.2.

## Exit codes: the whole protocol

Every command exits with a number: **0 means success**,
anything else means failure (1 is generic, others are
command-specific). `$?` holds the last one:

```bash
grep -q "x" file.txt
echo $?   # 0 if found, 1 if not
```

`if`, `while`, `&&`, and `||` all test exit codes — that's the
entire branching mechanism. There are no exceptions, no error
objects. Just numbers.

## set -euo pipefail: the three lines every script needs

```bash
#!/usr/bin/env bash
set -euo pipefail
```

Put this at the top of every script. What each flag does:

- **`set -e`** — exit immediately if any command fails. Without
  it, Bash happily continues after errors, running later
  commands in a broken state.
- **`set -u`** — treat unset variables as errors. Catches typos
  (`$nmae`) and, critically, prevents `rm -rf "$dir/"` from
  becoming `rm -rf /` when `$dir` is empty.
- **`set -o pipefail`** — a pipeline fails if *any* stage fails,
  not just the last. Without it, `generate_data | upload` reports
  success even when generation failed.

The classic horror story these prevent:

```bash
dir="/tmp/work"
rm -rf "$dir/"   # if dir were unset: rm -rf /  — set -u stops this
```

## Deliberate failure

```bash
[[ -f config.ini ]] || { echo "error: config.ini missing" >&2; exit 1; }
```

- Print errors to **stderr** (`>&2`), not stdout — keeps error
  text out of pipelines and captured output.
- `exit 1` (or another non-zero code) signals failure to callers.
- `{ ...; }` groups commands; note the required spaces and final
  `;`.

For functions, `return 1` instead of `exit 1` (exit kills the
whole script).

## Traps: cleanup on exit

```bash
tmpdir=$(mktemp -d)
trap 'rm -rf "$tmpdir"' EXIT
```

`trap ... EXIT` runs the command when the script exits — for any
reason, including errors. It's how you clean up temp files
reliably. (Quote carefully: single quotes delay expansion until
the trap fires, which is usually what you want.)

## set -e caveats (know these)

`set -e` is ignored in a few contexts — most notably the
condition of an `if`/`while` (that's fine, you're handling it)
and commands joined with `&&`/`||` (also fine). The practical
rule: don't fight it, and never write `set -e` scripts that
depend on subtle behavior — keep error paths explicit with
`|| exit 1`.

## Logging pattern

```bash
log()  { echo "[$(date +%T)] $*"; }
err()  { echo "[$(date +%T)] ERROR: $*" >&2; }
die()  { err "$*"; exit 1; }

[[ -f "$1" ]] || die "input file missing: $1"
```

Small helpers, used everywhere. `die` is the standard name for
"print error and exit."

## Check

- [ ] Every script you write starts with `set -euo pipefail`
- [ ] You can explain what each of the three flags prevents
- [ ] Errors go to stderr and failures `exit` non-zero

Next: [Troubleshooting](troubleshooting.md).
