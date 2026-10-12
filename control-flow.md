# Control Flow

**Scope:** `if`, loops, and `case`. All examples were run with
Bash 5.2.

## if: [[ ]] not [ ]

```bash
age=20
if [[ $age -ge 18 ]]; then
  echo "adult"
elif [[ $age -ge 13 ]]; then
  echo "teenager"
else
  echo "child"
fi
```

Use **`[[ ]]`** (double brackets), not the old `[ ]`:

- `[[ ]]` is Bash-native: no word-splitting surprises, supports
  `&&`/`||` inside, and `=~` regex matching.
- Numeric comparisons: `-eq` `-ne` `-lt` `-le` `-gt` `-ge`.
- String comparisons: `==` `!=`, and `-z` (empty) / `-n`
  (non-empty).

```bash
name="Ada"
if [[ -z $name ]]; then echo "no name"; fi
if [[ $name == A* ]]; then echo "starts with A"; fi  # glob pattern!
```

File tests you'll use constantly: `-f` (regular file exists),
`-d` (directory exists), `-r`/`-w`/`-x` (readable/writable/
executable).

## while: read files line by line

```bash
while IFS= read -r line; do
  echo "got: $line"
done < notes.txt
```

This is *the* idiom for processing a file line by line.
`IFS= read -r` preserves leading whitespace and backslashes —
copy it verbatim until you know why each piece is there.

Never do `for line in $(cat file)` — it word-splits and globs.

## for: two flavors

```bash
# Over items (glob, list, command output via array)
for f in *.txt; do
  echo "file: $f"
done

# C-style counting
for ((i = 0; i < 5; i++)); do
  echo "$i"
done
```

`for f in *.txt` with no matches: by default the pattern stays
literal (`*.txt`). Add `shopt -s nullglob` to make it expand to
nothing instead.

## case: Bash's switch

```bash
day="Sat"
case $day in
  Sat|Sun) echo "weekend" ;;
  Mon)     echo "back to work" ;;
  *)       echo "weekday" ;;
esac
```

Patterns are globs (`*.log`, `y|yes`), `;;` ends each branch,
`*)` is the default. Cleaner than `if` chains for matching
text.

## && and ||: the short-circuit idioms

```bash
[[ -f data.txt ]] && echo "exists"
mkdir -p build || exit 1
```

`&&` runs the right side only if the left succeeded; `||` only
if it failed. Great for one-liners — but don't chain them into
pseudo-if-statements; use a real `if` when logic gets complex.

## select: quick menus (bonus)

```bash
select opt in "start" "stop" "quit"; do
  echo "you chose: $opt"
  break
done
```

Bash builds a numbered menu for free. Handy for interactive
helper scripts.

## Check

- [ ] You use `[[ ]]` and can name two reasons it's better
- [ ] You can write the `while read` loop from memory
- [ ] You know why `for line in $(cat file)` is wrong

Next: [Functions and Arguments](functions-and-arguments.md).
