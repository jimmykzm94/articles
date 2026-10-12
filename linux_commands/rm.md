# Linux Command Cheatsheet: `rm`

`rm` removes files and directories.

## Basic Syntax

```bash
rm [options] file
```

Example:

```bash
rm file.txt
```

---

## Remove a File

```bash
rm file.txt
```

Remove multiple files:

```bash
rm file1.txt file2.txt file3.txt
```

---

## Remove Files Using Wildcards

Remove all `.log` files:

```bash
rm *.log
```

Remove all `.tmp` files:

```bash
rm *.tmp
```

Be careful with wildcards because they may match more files than expected.

---

## Ask Before Removing

```bash
rm -i file.txt
```

Example prompt:

```text
rm: remove regular file 'file.txt'?
```

---

## Force Removal

```bash
rm -f file.txt
```

`-f`:

- ignores nonexistent files
- does not ask for confirmation
- suppresses some error messages

Example:

```bash
rm -f *.tmp
```

---

## Remove a Directory

`rm` does not remove directories by default.

```bash
rm directory/
```

Usually returns:

```text
rm: cannot remove 'directory/': Is a directory
```

Use recursive mode:

```bash
rm -r directory/
```

---

## Remove Directory Recursively

```bash
rm -r project/
```

This removes:

```text
project/
project/file1.txt
project/file2.txt
project/src/
project/src/main.c
```

---

## Force Recursive Removal

```bash
rm -rf directory/
```

Commonly used to remove directories without confirmation.

Example:

```bash
rm -rf build/
```

Be very careful with `-rf`.

---

## Remove Multiple Directories

```bash
rm -r dir1 dir2 dir3
```

Force removal:

```bash
rm -rf dir1 dir2 dir3
```

---

## Verbose Output

Show what is being removed:

```bash
rm -v file.txt
```

Example:

```text
removed 'file.txt'
```

Recursive verbose removal:

```bash
rm -rv directory/
```

---

## Interactive Recursive Removal

```bash
rm -ri directory/
```

`rm` asks before removing files and directories.

Useful when deleting important directories.

---

## Prompt Once Before Large Removal

GNU `rm`:

```bash
rm -I *.log
```

`-I` asks once when:

- removing more than three files
- removing recursively

Example:

```bash
rm -rI project/
```

This is less intrusive than `-i`.

---

## Remove Empty Directories

Although `rm` can remove directories recursively:

```bash
rm -r empty_directory/
```

For empty directories, `rmdir` is safer:

```bash
rmdir empty_directory/
```

---

## Remove Hidden Files

Example:

```bash
rm .config
```

Remove hidden files matching a pattern:

```bash
rm .*.tmp
```

Be careful with hidden-file wildcards.

---

## Remove Files Beginning with `-`

A filename beginning with `-` may be interpreted as an option.

Example filename:

```text
-file.txt
```

Use:

```bash
rm -- -file.txt
```

Or:

```bash
rm ./-file.txt
```

`--` means the end of command options.

---

## Remove a File by Full Path

```bash
rm /home/user/logs/app.log
```

Remove several files:

```bash
rm /tmp/file1 /tmp/file2
```

---

## Remove Files from a Directory

```bash
rm logs/*.log
```

This removes matching files but keeps the `logs/` directory.

---

## Remove Directory Contents

```bash
rm -rf directory/*
```

Important: `*` normally does not match hidden files.

For a complete directory reset, removing and recreating the directory is often clearer:

```bash
rm -rf directory
mkdir directory
```

---

## Remove Files Found by `find`

Delete `.tmp` files:

```bash
find . -type f -name "*.tmp" -exec rm {} +
```

Or more directly:

```bash
find . -type f -name "*.tmp" -delete
```

---

## Remove Old Files

Remove `.log` files older than 30 days:

```bash
find /var/log -type f -name "*.log" -mtime +30 -delete
```

Always test the search first:

```bash
find /var/log -type f -name "*.log" -mtime +30
```

Then add `-delete`.

---

## Remove Build Files

Common example:

```bash
rm -rf build/
```

Other generated directories:

```bash
rm -rf dist/
```

```bash
rm -rf out/
```

---

## Remove Object Files

```bash
rm *.o
```

Recursively:

```bash
find . -type f -name "*.o" -delete
```

---

## Remove Multiple File Types

Using shell expansion:

```bash
rm *.o *.log *.tmp
```

If one pattern has no matches, shell behavior may vary.

Using `find` is more predictable for recursive cleanup:

```bash
find . -type f \( \
  -name "*.o" \
  -o -name "*.log" \
  -o -name "*.tmp" \
\) -delete
```

---

## Protect the Root Directory

GNU `rm` normally protects `/`.

This dangerous command:

```bash
rm -rf /
```

is normally rejected because of `--preserve-root`.

Do not disable this protection unless there is an extremely specific reason.

---

## `--preserve-root`

```bash
rm --preserve-root -rf /
```

GNU `rm` uses root protection by default.

Avoid:

```bash
rm --no-preserve-root -rf /
```

This can destroy the operating system.

---

## Common Options

| Option | Description |
|---|---|
| `-f` | Force removal |
| `-i` | Ask before every removal |
| `-I` | Ask once before large/recursive removal |
| `-r` | Remove directories recursively |
| `-R` | Same as `-r` |
| `-v` | Show removed files |
| `--` | End command options |
| `--preserve-root` | Protect `/` from recursive deletion |

---

## Common Option Combinations

Interactive recursive removal:

```bash
rm -ri directory/
```

Force recursive removal:

```bash
rm -rf directory/
```

Verbose recursive removal:

```bash
rm -rv directory/
```

Force recursive removal with output:

```bash
rm -rfv directory/
```

Prompt once before recursive removal:

```bash
rm -rI directory/
```

---

## Common Command Patterns

```bash
# Remove one file
rm file.txt

# Remove multiple files
rm file1.txt file2.txt

# Remove all log files
rm *.log

# Ask before deleting
rm -i file.txt

# Force deletion
rm -f file.txt

# Remove directory recursively
rm -r directory/

# Force recursive deletion
rm -rf directory/

# Show deleted files
rm -v *.log

# Interactive recursive deletion
rm -ri directory/

# Remove filename beginning with "-"
rm -- -file.txt

# Remove build directory
rm -rf build/

# Delete old log files
find . -type f -name "*.log" -mtime +30 -delete
```

## Important Notes

- `rm` normally deletes files permanently; there is usually no recycle bin.
- `-r` is required to remove directories and their contents.
- `-f` disables confirmation, so use it carefully.
- `rm -rf` is powerful and can destroy large amounts of data very quickly.
- Check the current directory with `pwd` before running destructive commands.
- Use `ls` first when using wildcards:

```bash
ls *.log
```

Then, if the result is correct:

```bash
rm *.log
```

- Use `--` when a filename begins with `-`.
- For scripts, validate variables before using them in an `rm -rf` command.