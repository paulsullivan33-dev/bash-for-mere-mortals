# Variables and Quoting

**Scope:** assignment, expansion, and the quoting rules that
prevent most Bash bugs. All examples were run with Bash 5.2.

## Assignment: no spaces

```bash
name="Ada"
age=36
```

**No spaces around `=`.** `name = "Ada"` tries to run a command
called `name` — the single most common beginner error, and the
error message won't hint at the cause.

## Expansion: $name

```bash
echo "Hello, $name"
echo "Next year: $((age + 1))"
```

- `$name` / `${name}` — the value. Braces disambiguate:
  `"${name}s"` vs `"$names"` (different variables!).
- `$((...))` — arithmetic.
- `$(...)` — command substitution (output of a command).

## The quoting rules (memorize these)

| Quote | Expands `$var`? | Expands `$(cmd)`? | Use for |
|---|---|---|---|
| `"double"` | Yes | Yes | Almost everything |
| `'single'` | No | No | Literal text |
| none | Yes, then word-split | Yes | Almost never |

```bash
file="my notes.txt"
cat "$file"    # one argument: my notes.txt  ✓
cat $file      # TWO arguments: my, notes.txt  ✗
```

Unquoted variables get **word-split** on spaces — the classic
source of "works until a filename has a space." **Rule: quote
every expansion, every time**, unless you specifically want
splitting or globbing.

Single quotes are for literals: `'Cost: $5'` prints `Cost: $5`
with no expansion.

## Special variables

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1`, `$2`, ... | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments (quoted: `"$@"`) |
| `$?` | Exit code of the last command |
| `$$` | This script's process ID |

```bash
#!/usr/bin/env bash
echo "Script: $0"
echo "Args: $#"
echo "First: $1"
```

Run `./args.sh one two` → `Script: ./args.sh`, `Args: 2`,
`First: one`.

**Always `"$@"`, never `$@` or `$*`**, when passing arguments
along — only `"$@"` preserves arguments containing spaces.

## Exports and environment

```bash
export EDITOR="nano"   # visible to child processes
EDITOR="nano"          # visible only in this shell
```

`export` marks a variable for child processes. Scripts usually
don't need it unless calling programs that read the variable.

## readonly and defaults

```bash
readonly VERSION="1.0"              # cannot be reassigned
echo "${MISSING:-default}"          # "default" if unset
echo "${MISSING:?must be set}"      # error out if unset
```

`${var:-default}` is handy for optional settings;
`${var:?message}` fails fast on required ones.

## Check

- [ ] You write `name="x"` with no spaces and can say why
- [ ] You quote every `"$variable"` by reflex
- [ ] You can explain `"$@"` vs `$*` in one sentence

Next: [Control Flow](control-flow.md).
