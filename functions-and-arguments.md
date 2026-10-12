# Functions and Arguments

**Scope:** defining functions, parameters, return values, and
parsing script arguments. All examples were run with Bash 5.2.

## Defining and calling

```bash
greet() {
  echo "Hello, $1!"
}

greet "Ada"   # Hello, Ada!
```

- Define with `name() { ...; }` — the `function` keyword also
  works but the paren form is standard.
- **Call with spaces, not parens:** `greet "Ada"`, never
  `greet("Ada")` (that's a syntax error).

## Parameters inside functions

```bash
deploy() {
  local env="$1"
  local version="${2:-latest}"
  echo "Deploying $version to $env"
}
```

- `$1`, `$2`, ... are the function's arguments (shadowing the
  script's).
- **`local`** keeps variables inside the function — without it,
  every variable is global. Always use `local`.
- `${2:-latest}` gives `$2` a default.

## Return values: exit codes, not data

Bash functions "return" an **exit code** (0–255):

```bash
is_root() {
  [[ $EUID -eq 0 ]]
}
if is_root; then echo "running as root"; fi
```

To return *data*, print it and capture with `$(...)`:

```bash
today() {
  date +%F
}
stamp=$(today)
```

This is the fundamental Bash pattern: functions communicate via
stdout, success/failure via exit code.

## Parsing arguments: getopts

```bash
#!/usr/bin/env bash
verbose=0
while getopts "v" opt; do
  case $opt in
    v) verbose=1 ;;
  esac
done
shift $((OPTIND - 1))

[[ $verbose -eq 1 ]] && echo "verbose on"
echo "remaining args: $*"
```

`getopts "v"` accepts `-v`; `"v:"` (colon) means `-v` takes a
value, available as `$OPTARG`. `shift $((OPTIND - 1))` drops the
parsed flags so `$@` holds the real arguments. For anything
fancier (long options), parse manually or reconsider Python.

## Sourcing: sharing code between scripts

```bash
# lib.sh
log() { echo "[$(date +%T)] $*"; }
```

```bash
#!/usr/bin/env bash
source ./lib.sh   # or: . ./lib.sh
log "starting"
```

`source` runs another file *in the current shell* — its
functions and variables become yours. It's how you build a
little library. (Note: sourced files share everything, including
accidental globals — another reason for `local`.)

## The main-function pattern

```bash
#!/usr/bin/env bash
set -euo pipefail

main() {
  local env="${1:-dev}"
  deploy "$env"
}

main "$@"
```

Putting logic in `main` and calling it at the bottom keeps
scripts organized and makes the flow obvious. (`set -euo
pipefail` is explained in [Error Handling](error-handling.md) —
put it in every script.)

## Check

- [ ] You define with `name() { }`, call with spaces, use `local`
- [ ] You return data via stdout and status via exit code
- [ ] You can parse a `-v` flag with `getopts`

Next: [Pipes and Redirection](pipes-and-redirection.md).
