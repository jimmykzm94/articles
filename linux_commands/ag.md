# Linux Command Cheatsheet: `ag`

`ag`, also called **The Silver Searcher**, is a fast recursive text-search tool commonly used for searching source code.

## Basic Syntax

```bash
ag [options] pattern [path]
```

Example:

```bash
ag "printf"
```

Search inside a specific directory:

```bash
ag "printf" src/
```

---

## Basic Search

Search recursively from the current directory:

```bash
ag "TODO"
```

Search in another directory:

```bash
ag "TODO" project/
```

Search a specific file:

```bash
ag "error" log.txt
```

---

## Ignore Case

```bash
ag -i "error"
```

Matches:

```text
error
Error
ERROR
```

---

## Force Case-Sensitive Search

```bash
ag -s "Error"
```

Only matching capitalization is returned.

---

## Smart Case

```bash
ag -S "error"
```

Lowercase pattern:

```bash
ag -S "error"
```

is case-insensitive.

Pattern containing uppercase:

```bash
ag -S "Error"
```

becomes case-sensitive.

Smart case is enabled by default.

---

## Search Whole Words

```bash
ag -w "main"
```

Matches:

```text
main
```

Does not match:

```text
maintain
main_loop
```

---

## Literal Search

By default, patterns are regular expressions.

Search literal text:

```bash
ag -Q "hello.world"
```

Or:

```bash
ag -F "hello.world"
```

The `.` is treated as a normal character.

---

## Regular Expressions

Search alternatives:

```bash
ag "error|warning"
```

Match numbers:

```bash
ag "[0-9]+"
```

Search function-like patterns:

```bash
ag "printf\(.*\)"
```

---

## Show Only Matching Text

```bash
ag -o "[0-9]+"
```

Example input:

```text
Port 8080 failed
```

Output:

```text
8080
```

---

## Invert Match

Show lines that do not match:

```bash
ag -v "debug"
```

---

## Count Matches

```bash
ag -c "TODO"
```

Example:

```text
src/main.c:3
src/test.c:5
```

`-c` counts matches per file, not necessarily matching lines.

---

## Show Files Containing Matches

```bash
ag -l "TODO"
```

Example:

```text
src/main.c
src/test.c
```

---

## Show Files Without Matches

```bash
ag -L "TODO"
```

---

## Show Line Numbers

```bash
ag --numbers "error"
```

Line numbers are normally shown when searching files.

---

## Show Column Numbers

```bash
ag --column "error"
```

Example output:

```text
main.c:25:10:error occurred
```

---

## Context Around Matches

Show 3 lines after:

```bash
ag -A 3 "error"
```

Show 3 lines before:

```bash
ag -B 3 "error"
```

Show 3 lines before and after:

```bash
ag -C 3 "error"
```

Without a number, context defaults to 2 lines:

```bash
ag -C "error"
```

---

## Search Specific File Types

Search C files:

```bash
ag --cc "TODO"
```

Search C++ files:

```bash
ag --cpp "TODO"
```

Search Python files:

```bash
ag --python "TODO"
```

Search JavaScript files:

```bash
ag --js "TODO"
```

List supported file types:

```bash
ag --list-file-types
```

---

## Search Files by Filename Pattern

Use `-G`:

```bash
ag -G "\.c$" "TODO"
```

Search only C files.

Another example:

```bash
ag -G "test.*\.py$" "assert"
```

---

## Find Filenames

Use `-g`:

```bash
ag -g "main"
```

Example results:

```text
src/main.c
src/main.h
```

Find C files:

```bash
ag -g "\.c$"
```

This searches filenames instead of file contents.

---

## Search Hidden Files

```bash
ag --hidden "TODO"
```

Hidden files are normally excluded.

Example hidden files:

```text
.gitignore
.config
.env
```

---

## Search All Text Files

```bash
ag -t "TODO"
```

This searches all text files regardless of recognized file type.

Hidden files are still excluded.

---

## Search All Files

```bash
ag -a "TODO"
```

`-a`:

- searches all files
- does not respect ignore files
- still excludes hidden files

---

## Unrestricted Search

```bash
ag -u "TODO"
```

This searches:

- hidden files
- binary files
- ignored files
- files excluded by `.gitignore`

Use carefully on large directories.

---

## Ignore Files or Directories

Ignore one directory:

```bash
ag "TODO" --ignore build
```

Ignore file patterns:

```bash
ag "TODO" --ignore "*.log"
```

Ignore multiple patterns:

```bash
ag "TODO" \
  --ignore build \
  --ignore "*.log"
```

---

## `.gitignore` Support

`ag` automatically respects ignore files such as:

```text
.gitignore
.hgignore
.ignore
```

So directories such as:

```text
build/
node_modules/
generated/
```

may automatically be skipped if listed there.

---

## Ignore `.gitignore`

```bash
ag -U "TODO"
```

`-U` ignores VCS ignore files such as:

```text
.gitignore
.hgignore
```

but still respects `.ignore`.

---

## Use a Custom Ignore File

```bash
ag -p my-ignore-file "TODO"
```

Or:

```bash
ag --path-to-ignore my-ignore-file "TODO"
```

---

## Limit Search Depth

```bash
ag --depth 2 "TODO"
```

Search only two directory levels deep.

Unlimited:

```bash
ag --depth -1 "TODO"
```

---

## Do Not Recurse

```bash
ag -n "TODO"
```

Search only the current directory.

---

## Follow Symbolic Links

```bash
ag -f "TODO"
```

By default, symbolic links are not followed.

---

## Search Binary Files

```bash
ag --search-binary "pattern"
```

Binary files are normally ignored.

---

## Search Compressed Files

```bash
ag -z "error"
```

Can search supported compressed formats such as:

```text
.gz
.xz
```

Support depends on how `ag` was built.

---

## Limit Number of Matches

Stop searching each file after 5 matches:

```bash
ag -m 5 "error"
```

---

## Group Output

Default behavior groups matches by file:

```bash
ag "error"
```

Example:

```text
main.c
10:error occurred
25:error occurred
```

Disable grouping:

```bash
ag --nogroup "error"
```

Example:

```text
main.c:10:error occurred
main.c:25:error occurred
```

---

## Output for Vim

```bash
ag --vimgrep "TODO"
```

Output includes:

```text
filename:line:column:match
```

Useful for Vim quickfix integration.

---

## Null-Separated Filenames

```bash
ag -l -0 "TODO"
```

Useful with:

```bash
xargs -0
```

Example:

```bash
ag -l -0 "TODO" | xargs -0 sed -i 's/TODO/DONE/g'
```

This safely handles filenames containing spaces.

---

## Show Statistics

```bash
ag --stats "TODO"
```

Shows information such as:

- files searched
- matches found
- execution time

Statistics only:

```bash
ag --stats-only "TODO"
```

---

## Search Command Output

`ag` can search stdin:

```bash
dmesg | ag "error"
```

Example:

```bash
ps aux | ag "python"
```

---

## Passthrough Mode

Print all stdin lines while highlighting matches:

```bash
cat log.txt | ag --passthrough "error"
```

Non-matching lines are still printed.

---

## Search for Patterns Beginning with `-`

Example pattern:

```text
--help
```

Use:

```bash
ag -- "--help"
```

`--` tells `ag` that remaining arguments are not command options.

---

## Common Options

| Option | Description |
|---|---|
| `-i` | Ignore case |
| `-s` | Case-sensitive search |
| `-S` | Smart case |
| `-w` | Match whole words |
| `-Q` | Literal search |
| `-F` | Literal/fixed-string search |
| `-v` | Invert match |
| `-o` | Show only matching text |
| `-c` | Count matches |
| `-l` | Show files with matches |
| `-L` | Show files without matches |
| `-A N` | Show N lines after |
| `-B N` | Show N lines before |
| `-C N` | Show context |
| `-G` | Restrict search by filename regex |
| `-g` | Search filenames |
| `-t` | Search all text files |
| `-a` | Search all files |
| `-u` | Unrestricted search |
| `-U` | Ignore VCS ignore files |
| `--hidden` | Include hidden files |
| `--ignore` | Ignore file/directory pattern |
| `--depth` | Limit directory depth |
| `-n` | Disable recursive search |
| `-f` | Follow symbolic links |
| `-m N` | Maximum matches per file |
| `--column` | Show column number |
| `--stats` | Show search statistics |
| `--vimgrep` | Vim-compatible output |
| `-0` | Null-separated filenames |

---

## Common Command Patterns

```bash
# Basic search
ag "TODO"

# Search directory
ag "TODO" src/

# Ignore case
ag -i "error"

# Whole word
ag -w "main"

# Literal search
ag -Q "hello.world"

# Search C files
ag --cc "TODO"

# Search files matching filename regex
ag -G "\.c$" "TODO"

# Find filenames
ag -g "main"

# Search hidden files
ag --hidden "TODO"

# Ignore build directory
ag "TODO" --ignore build

# Show 3 lines of context
ag -C 3 "error"

# List files containing pattern
ag -l "TODO"

# Search everything
ag -u "TODO"

# Vim-friendly output
ag --vimgrep "TODO"
```

## `ag` vs `grep`

`grep`:

```bash
grep -rn "TODO" .
```

`ag`:

```bash
ag "TODO"
```

`ag` recursively searches directories by default and automatically respects common ignore files such as `.gitignore`.

## Important Notes

- `ag` recursively searches directories by default.
- It is designed mainly for fast source-code and text searching.
- `.gitignore`, `.hgignore`, and `.ignore` rules are respected by default.
- Binary files are ignored by default.
- Smart-case searching is enabled by default.
- Use `-Q` when the search text contains regex characters but should be treated literally.
- Use `-g` when you want to search filenames instead of file contents.
- Use `-G` when you want to restrict content searches to matching filenames.
- `-u` can make searches much larger because ignored, hidden, and binary files are included.
- `ag --list-file-types` shows the file-type shortcuts supported by your installed version.