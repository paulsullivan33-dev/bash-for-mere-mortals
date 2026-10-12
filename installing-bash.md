# Installing Bash

**Scope:** confirming Bash is present and new enough, with notes
for Windows and Mac. On most systems this is a check, not an
install.

## Linux

It's already there. Check the version:

```sh
bash --version | head -1
```

Anything 4.x or 5.x is fine for these guides.

## Mac

Bash is preinstalled — but it's **version 3.2 from 2007**, kept
for licensing reasons. It lacks modern features (associative
arrays, `mapfile`, `**` globbing). For these guides, install a
current Bash via Homebrew:

```sh
brew install bash
```

Then check `bash --version` gives 5.x. (Your login shell stays
3.2 unless you change it — scripts just need the `#!/usr/bin/env
bash` shebang to find the new one via PATH.)

## Windows

Two good options:

- **Git Bash** — comes with Git for Windows; a perfectly usable
  Bash for scripting.
- **WSL2** — a real Linux inside Windows; the full experience.

PowerShell is a different language entirely — these guides don't
apply to it.

## The one check that matters

```sh
echo $BASH_VERSION
```

If it prints something like `5.2.21(1)-release`, you're set. If
it prints nothing, you're not running Bash (maybe `sh`, `zsh`,
or PowerShell) — switch to Bash before continuing.

## sh vs bash: know the difference

`/bin/sh` is *not* Bash on many systems (on Ubuntu it's Dash, a
smaller shell). Scripts starting with `#!/bin/sh` must avoid
Bash-only features. These guides target Bash specifically — use
the `#!/usr/bin/env bash` shebang (see
[Your First Script](first-script.md)).

## Check

- [ ] `echo $BASH_VERSION` prints a version (4.x+ ideally)
- [ ] You can say why Mac's default Bash is avoided
- [ ] You know the difference between `sh` and `bash`

Next: [Your First Script](first-script.md).
