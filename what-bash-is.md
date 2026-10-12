# What Bash Is

**Scope:** what Bash is, where it's used, and whether it's worth
your time. Not a tutorial — that starts with
[Your First Script](first-script.md).

## The one-sentence version

Bash is both a command interpreter (the thing behind your
terminal prompt) and a scripting language for automating
everything you can type.

## The blurry line that matters

Most languages separate "using" from "programming." Bash doesn't:
every command you type is already a Bash program, one line long.
A script is just those lines saved in a file. This is why Bash is
the fastest path from "I do this by hand" to "it runs itself" —
you're automating commands you already know.

## Where Bash actually lives

- **Servers:** startup scripts, cron jobs, deploy scripts,
  health checks. If it runs on Linux on a schedule, Bash is
  usually involved.
- **CI/CD:** GitHub Actions, Jenkins, GitLab CI — the `run:`
  blocks are shell scripts.
- **DevOps glue:** wrapping `docker`, `kubectl`, `ansible`,
  `rsync` into repeatable workflows.
- **Your own machine:** renaming 500 files, converting formats,
  scraping logs — the one-off jobs too small for a "real"
  program.

## What Bash is not

- **Not a general-purpose language.** Anything over ~100 lines,
  with complex data or real error handling, wants Python or Go.
  Bash has no proper data structures — everything is strings.
- **Not safe by default.** Unquoted variables, silent failures,
  and `rm -rf` with an empty variable are all one typo away.
  Safety is opt-in (see [Error Handling](error-handling.md)).
- **Not portable without care.** Mac ships ancient Bash 3.2;
  Linux has Bash 5.x; Windows needs Git Bash or WSL. Features
  differ between them.
- **Not fast at computation.** Loops in Bash are slow; heavy
  lifting belongs in `awk` or another language.

## The 100-line rule

A useful rule of thumb from graybeards: if your script is
approaching 100 lines, or needs arrays of structured data,
nested quoting, or real error recovery — rewrite it in Python.
Bash is for gluing commands together, not for building
applications.

## Should you learn it?

Worth it if you:

- Touch Linux servers, Docker, or CI pipelines (so: yes, for
  DevOps work)
- Want to automate repetitive terminal tasks
- Read other people's scripts and want to understand them

You already use it every time you open a terminal — learning it
properly just turns typing into scripting.

## Check

You can explain, in your own words:

- [ ] Why a terminal command and a Bash script are the same thing
- [ ] The 100-line rule and the reasoning behind it
- [ ] One thing Bash is bad at, and what to use instead

Next: [Installing Bash](installing-bash.md).
