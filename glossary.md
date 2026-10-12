# Glossary

Quick definitions, A to Z. Each term is explained in depth in its
guide; this is the reminder card.

- **Argument** — input to a command or script (`$1`, `$2`, ...);
  all args as `"$@"`.
- **Command substitution** — `$(...)`: runs a command, yields
  its output.
- **Exit code** — a command's numeric result; 0 is success
  (`$?` holds the last one).
- **Expansion** — Bash replacing `$var`, `$(cmd)`, globs, etc.
  before running a command.
- **Glob** — filename pattern (`*.txt`, `?.log`); expanded by
  the shell, not the command.
- **Heredoc** — multi-line input fed to a command
  (`<< 'EOF'`).
- **Parameter expansion** — `${var...}` string surgery:
  defaults, stripping, lengths.
- **Pipe** — `|`: connects one command's stdout to the next
  command's stdin.
- **PIPESTATUS** — array of every stage's exit code in the last
  pipeline.
- **Redirection** — `>`, `>>`, `<`, `2>`: sending streams to
  files or merging them.
- **Shebang** — the `#!/usr/bin/env bash` first line naming the
  interpreter.
- **ShellCheck** — the linter that catches Bash mistakes;
  run it on everything.
- **Source** — `source file` / `. file`: run a file in the
  current shell (shares functions/variables).
- **stderr** — stream 2: errors and diagnostics.
- **stdin** — stream 0: a command's input.
- **stdout** — stream 1: a command's normal output.
- **Subshell** — a child shell copy; variable changes inside
  don't escape (pipeline stages run in subshells).
- **Word splitting** — unquoted expansions split on spaces;
  the reason you quote `"$var"`.
- **set -euo pipefail** — the three flags every script starts
  with: exit on error, error on unset variables, pipelines fail
  if any stage fails.
- **trap** — run a command on exit/signal; used for cleanup.
- **xargs** — builds command arguments from stdin lines.
