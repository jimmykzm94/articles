# Linux Command Cheatsheet: `sed`

`sed` is a stream editor used to search, replace, delete, insert, and transform text.

## Basic Syntax

```bash
sed [options] 'command' file
```

By default, `sed` prints the result without changing the original file.

---

## Replace Text

Replace the first match on each line:

```bash
sed 's/old/new/' file.txt
```

Replace every match:

```bash
sed 's/old/new/g' file.txt
```

Replace only the second match:

```bash
sed 's/old/new/2' file.txt
```

Case-insensitive replacement with GNU `sed`:

```bash
sed 's/error/warning/gI' file.txt
```

---

## Edit a File In Place

```bash
sed -i 's/old/new/g' file.txt
```

Create a backup before editing:

```bash
sed -i.bak 's/old/new/g' file.txt
```

macOS/BSD `sed` requires an extension after `-i`:

```bash
sed -i '' 's/old/new/g' file.txt
```

---

## Use a Different Delimiter

Useful when replacing paths or URLs:

```bash
sed 's|/old/path|/new/path|g' file.txt
```

Other characters can also be used:

```bash
sed 's#https://old.com#https://new.com#g' file.txt
```

---

## Print Specific Lines

Print line 5:

```bash
sed -n '5p' file.txt
```

Print lines 5 through 10:

```bash
sed -n '5,10p' file.txt
```

Print the last line:

```bash
sed -n '$p' file.txt
```

Print lines matching a pattern:

```bash
sed -n '/error/p' file.txt
```

---

## Delete Lines

Delete line 5:

```bash
sed '5d' file.txt
```

Delete lines 5 through 10:

```bash
sed '5,10d' file.txt
```

Delete the last line:

```bash
sed '$d' file.txt
```

Delete empty lines:

```bash
sed '/^$/d' file.txt
```

Delete lines containing a word:

```bash
sed '/debug/d' file.txt
```

Delete comment lines:

```bash
sed '/^[[:space:]]*#/d' file.txt
```

---

## Insert and Append Text

Insert text before line 3:

```bash
sed '3i\New text' file.txt
```

Append text after line 3:

```bash
sed '3a\New text' file.txt
```

Insert before matching lines:

```bash
sed '/error/i\Warning:' file.txt
```

Append after matching lines:

```bash
sed '/error/a\Check this line' file.txt
```

---

## Replace an Entire Line

Replace line 3:

```bash
sed '3c\Replacement text' file.txt
```

Replace lines matching a pattern:

```bash
sed '/localhost/c\127.0.0.1 example.com' file.txt
```

---

## Work with Line Ranges

Replace text only between lines 5 and 10:

```bash
sed '5,10s/old/new/g' file.txt
```

Delete text between two patterns:

```bash
sed '/START/,/END/d' file.txt
```

Print text between two patterns:

```bash
sed -n '/START/,/END/p' file.txt
```

---

## Remove Whitespace

Remove leading whitespace:

```bash
sed 's/^[[:space:]]*//' file.txt
```

Remove trailing whitespace:

```bash
sed 's/[[:space:]]*$//' file.txt
```

Remove both:

```bash
sed 's/^[[:space:]]*//; s/[[:space:]]*$//' file.txt
```

Replace multiple spaces with one space:

```bash
sed 's/[[:space:]]\+/ /g' file.txt
```

For portable extended patterns:

```bash
sed -E 's/[[:space:]]+/ /g' file.txt
```

---

## Capture Groups

Swap two words:

```bash
echo "hello world" | sed -E 's/(hello) (world)/\2 \1/'
```

Output:

```text
world hello
```

Wrap matching text:

```bash
sed -E 's/([0-9]+)/[\1]/g' file.txt
```

---

## Add Text to Every Line

Add a prefix:

```bash
sed 's/^/PREFIX: /' file.txt
```

Add a suffix:

```bash
sed 's/$/ :SUFFIX/' file.txt
```

Add line numbers:

```bash
sed = file.txt | sed 'N; s/\n/ /'
```

---

## Multiple Commands

Use multiple `-e` options:

```bash
sed -e 's/foo/bar/g' -e '/debug/d' file.txt
```

Or separate commands with semicolons:

```bash
sed 's/foo/bar/g; /debug/d' file.txt
```

Use a script file:

```bash
sed -f commands.sed file.txt
```

Example `commands.sed`:

```text
s/foo/bar/g
/debug/d
```

---

## Use with Pipes

Replace text from another command:

```bash
printf '%s\n' "hello world" | sed 's/world/Linux/'
```

Filter command output:

```bash
ps aux | sed -n '1,5p'
```

Remove comments and empty lines:

```bash
cat config.conf |
  sed '/^[[:space:]]*#/d; /^[[:space:]]*$/d'
```

---

## Common Options

| Option | Description |
|---|---|
| `-n` | Disable automatic output |
| `-i` | Edit files in place |
| `-e` | Add a command |
| `-f` | Read commands from a file |
| `-E` | Use extended regular expressions |

---

## Common Patterns

```bash
# Replace all matches
sed 's/old/new/g' file.txt

# Edit in place with backup
sed -i.bak 's/old/new/g' file.txt

# Print matching lines
sed -n '/pattern/p' file.txt

# Delete matching lines
sed '/pattern/d' file.txt

# Print a line range
sed -n '10,20p' file.txt

# Remove empty lines
sed '/^[[:space:]]*$/d' file.txt

# Remove comments
sed '/^[[:space:]]*#/d' file.txt

# Trim whitespace
sed 's/^[[:space:]]*//; s/[[:space:]]*$//' file.txt

# Replace only on matching lines
sed '/pattern/s/old/new/g' file.txt
```

## Important Notes

- `sed` does not modify files unless `-i` is used.
- Use `-n` together with `p` to print only selected lines.
- Use single quotes to prevent the shell from expanding special characters.
- Test commands without `-i` before changing important files.
- GNU and macOS/BSD versions of `sed` have small syntax differences.