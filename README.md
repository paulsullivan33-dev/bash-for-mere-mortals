# Bash for Mere Mortals

Plain-language guides to Bash — the shell behind most of the
world's servers, scripts, and automation glue — written for
people who live in terminals but never formally learned the
language. No CS degree required.

No jargon without an explanation. No "it's simple, just..." Every
concept starts with the *idea*, then the commands.

One honest note up front: Bash is not a designed language so much
as forty years of accreted history. Quoting rules are subtle,
error handling is opt-in, and whitespace can be load-bearing.
These guides teach the safe modern subset and mark the traps —
because a Bash script that "mostly works" is how 2 AM pages are
born.

## Start here

1. [What Bash Is](what-bash-is.md) — what it is, where it lives,
   and why learn it.
2. [Installing Bash](installing-bash.md) — it's probably already
   there; Windows and Mac notes included.
3. [Your First Script](first-script.md) — shebang, `chmod +x`,
   and running it, with checks along the way.

## The building blocks

4. [Variables and Quoting](variables-and-quoting.md) — the
   quoting rules that prevent 90% of Bash bugs.
5. [Control Flow](control-flow.md) — `if`, loops, and `case`.
6. [Functions and Arguments](functions-and-arguments.md) —
   organizing scripts and handling inputs.
7. [Pipes and Redirection](pipes-and-redirection.md) — stdin,
   stdout, stderr, and chaining commands.
8. [Text Processing](text-processing.md) — `grep`, `sed`, `awk`:
   the classic trio.
9. [Error Handling](error-handling.md) — `set -euo pipefail` and
   exit codes: making scripts fail loudly.

## Reference

10. [Troubleshooting](troubleshooting.md) — error messages and
    common gotchas, plain fixes.
11. [Glossary](glossary.md) — quick definitions, A to Z.

## Writing criteria

Every guide follows the same [writing criteria](criteria.md): plain
words, no hype, honest limits, and checkable steps.
