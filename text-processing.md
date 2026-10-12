# Text Processing

**Scope:** `grep`, `sed`, and `awk` — the classic trio for
searching and reshaping text. All examples were run with Bash
5.2 (GNU grep/sed/awk).

## grep: find lines

```bash
grep "error" app.log          # lines containing "error"
grep -i "error" app.log       # case-insensitive
grep -v "debug" app.log       # lines NOT containing "debug"
grep -r "TODO" src/           # recursive through a directory
grep -c "404" access.log      # count matching lines
```

`grep` exits 0 if anything matched, 1 if nothing did — so it
works directly in `if`:

```bash
if grep -q "FATAL" app.log; then
  echo "found a fatal error"
fi
```

`-q` means quiet (no output, just the exit code).

**Regular expressions** (patterns, not literal text):

```bash
grep -E "^2026-10-12.*ERROR" app.log  # -E: extended regex
```

`^` anchors to line start, `.*` means "anything." `-E` enables
the modern regex syntax — use it by default.

## sed: edit streams

`sed` transforms text line by line. The famous use is
substitution:

```bash
sed 's/old/new/' file.txt        # first occurrence per line
sed 's/old/new/g' file.txt       # all occurrences (g = global)
sed -i 's/old/new/g' file.txt    # -i: edit the file in place
```

**Back up before `-i`:** `sed -i.bak '...'` keeps `file.txt.bak`.
In-place editing with no backup is how data gets destroyed.

Other handy forms:

```bash
sed -n '10,20p' file.txt   # print lines 10–20
sed '/debug/d' file.txt    # delete lines containing "debug"
```

## awk: columns and computation

`awk` splits each line into fields (`$1`, `$2`, ...) on
whitespace and runs your program per line:

```bash
awk '{ print $1 }' access.log          # first column
awk '$3 > 100 { print $0 }' data.txt   # lines where column 3 > 100
awk -F: '{ print $1 }' /etc/passwd     # -F: sets the separator
```

Summing a column — the canonical awk one-liner:

```bash
awk '{ sum += $2 } END { print sum }' numbers.txt
```

`END` runs after the last line. `BEGIN` runs before the first
(useful for headers).

## Which tool when

| Job | Tool |
|---|---|
| Find lines matching a pattern | `grep` |
| Replace text in a stream/file | `sed 's///'` |
| Work with columns/fields | `awk` |
| Sort / unique / count | `sort`, `uniq -c` |
| Take the head/tail of output | `head`, `tail -f` |
| Anything needing real logic | Python, not Bash |

```bash
# The classics, composed:
grep " 404 " access.log | awk '{ print $7 }' | sort | uniq -c | sort -rn | head
# → top requested URLs that 404'd
```

## cut, tr, sort: the supporting cast

```bash
cut -d, -f2 data.csv     # second comma-separated field
tr 'a-z' 'A-Z' < f.txt   # uppercase everything
sort -u names.txt        # sort, dropping duplicates
```

## Check

- [ ] You can grep recursively, count matches, and use `-q` in `if`
- [ ] You can `sed 's/old/new/g'` with a backup
- [ ] You can print/sum a column with `awk`

Next: [Error Handling](error-handling.md).
