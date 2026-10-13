# Linux Command Cheatsheet: `mv`

`mv` moves files and directories. It is also commonly used to rename them.

## Basic Syntax

```bash
mv [options] source destination
```

Example:

```bash
mv file.txt backup/
```

---

## Move a File

```bash
mv file.txt backup/
```

Move using full paths:

```bash
mv /home/user/file.txt /home/user/backup/
```

---

## Move Multiple Files

```bash
mv file1.txt file2.txt file3.txt backup/
```

The last argument must be a directory.

---

## Move Files Using Wildcards

Move all `.log` files:

```bash
mv *.log logs/
```

Move all `.jpg` files:

```bash
mv *.jpg images/
```

Be careful with wildcards because they may match more files than expected.

---

## Rename a File

`mv` is also used to rename files:

```bash
mv old.txt new.txt
```

Example:

```bash
mv config.old config.conf
```

---

## Rename a Directory

```bash
mv old_directory new_directory
```

Example:

```bash
mv project project_backup
```

---

## Move a Directory

```bash
mv project/ archive/
```

After the command:

```text
archive/project/
```

---

## Move Multiple Directories

```bash
mv dir1 dir2 dir3 archive/
```

---

## Move and Rename at the Same Time

```bash
mv file.txt backup/new_name.txt
```

Example:

```bash
mv app.log logs/app-old.log
```

---

## Ask Before Overwriting

```bash
mv -i file.txt destination/
```

If the destination already exists, `mv` asks for confirmation.

Example:

```text
mv: overwrite 'destination/file.txt'?
```

---

## Force Overwrite

```bash
mv -f file.txt destination/
```

`-f` overwrites the destination without prompting.

---

## Never Overwrite Existing Files

```bash
mv -n file.txt destination/
```

If the destination file already exists, it is not replaced.

Useful when moving many files:

```bash
mv -n *.txt backup/
```

---

## Create a Backup Before Overwriting

GNU `mv`:

```bash
mv -b file.txt destination/
```

If the destination already contains:

```text
file.txt
```

the old file may be renamed to:

```text
file.txt~
```

before the new file replaces it.

---

## Specify Backup Suffix

```bash
mv -b --suffix=.bak file.txt destination/
```

Existing file becomes:

```text
file.txt.bak
```

---

## Verbose Output

Show what is being moved:

```bash
mv -v file.txt backup/
```

Example:

```text
renamed 'file.txt' -> 'backup/file.txt'
```

Move multiple files:

```bash
mv -v *.log logs/
```

---

## Move Only if Source Is Newer

GNU `mv`:

```bash
mv -u file.txt destination/
```

`-u` updates the destination only when:

- the destination does not exist, or
- the source is newer than the destination

Useful for synchronization-style operations.

---

## Move Files into Current Directory

```bash
mv /tmp/file.txt .
```

`.` means the current directory.

Move several files:

```bash
mv /tmp/*.log .
```

---

## Move Files into Parent Directory

```bash
mv file.txt ..
```

`..` means the parent directory.

---

## Move a File Up Multiple Levels

```bash
mv file.txt ../../
```

---

## Move Hidden Files

Move a specific hidden file:

```bash
mv .env backup/
```

Move hidden files matching a pattern:

```bash
mv .config* backup/
```

Be careful with shell wildcard behavior when working with dotfiles.

---

## Move a File Beginning with `-`

Example filename:

```text
-test.txt
```

Use:

```bash
mv -- -test.txt backup/
```

Or:

```bash
mv ./-test.txt backup/
```

`--` marks the end of command options.

---

## Move Files Found by `find`

Move all `.log` files into another directory:

```bash
find . -type f -name "*.log" -exec mv {} logs/ \;
```

More efficiently:

```bash
find . -type f -name "*.log" -exec mv -t logs/ {} +
```

`-t` is a GNU `mv` option.

---

## Specify Destination Directory First

GNU `mv`:

```bash
mv -t backup/ file1.txt file2.txt file3.txt
```

Equivalent to:

```bash
mv file1.txt file2.txt file3.txt backup/
```

This is useful with `find` and `xargs`.

---

## Move Files with `find` and `-t`

```bash
find . -type f -name "*.log" -exec mv -t logs/ {} +
```

---

## Move Files with `xargs`

```bash
find . -type f -name "*.log" -print0 |
  xargs -0 mv -t logs/
```

Using null-separated filenames safely handles spaces and unusual characters.

---

## Move Old Files

Move `.log` files older than 30 days:

```bash
find . -type f -name "*.log" -mtime +30 -exec mv -t archive/ {} +
```

Test first:

```bash
find . -type f -name "*.log" -mtime +30
```

---

## Move Large Files

Move files larger than 100 MB:

```bash
find . -type f -size +100M -exec mv -t large-files/ {} +
```

---

## Move Files by Extension

Move C source files:

```bash
mv *.c src/
```

Move header files:

```bash
mv *.h include/
```

Move object files:

```bash
mv *.o build/
```

---

## Move Files into Newly Created Directory

```bash
mkdir archive
mv *.log archive/
```

Or:

```bash
mkdir -p archive
mv *.log archive/
```

---

## Move Directory Contents

```bash
mv source/* destination/
```

Important: `*` normally does not include hidden files.

To move a whole directory instead:

```bash
mv source destination/
```

is often simpler.

---

## Move Without Accidentally Nesting a Directory

Suppose:

```text
destination/
```

already exists.

This:

```bash
mv project destination/
```

creates:

```text
destination/project/
```

If you intend to rename `project` instead, make sure the target name does not already exist.

---

## Use `-T` to Treat Destination as a Normal Path

GNU `mv`:

```bash
mv -T source destination
```

Normally:

```bash
mv source destination/
```

may place `source` inside `destination`.

With `-T`, `destination` is treated as the destination itself rather than as a directory to move into.

This is useful in scripts where nesting must be avoided.

---

## Prevent Moving into Itself

GNU `mv` supports:

```bash
mv -T source destination
```

which can make destination behavior more explicit in scripts.

---

## Cross-Filesystem Moves

When moving within the same filesystem, `mv` can usually rename the filesystem entry directly.

Example:

```bash
mv file.txt backup/
```

This is usually very fast.

When moving across different filesystems or mounted drives, `mv` may need to:

1. Copy the file
2. Preserve applicable metadata
3. Remove the original

Example:

```bash
mv large.iso /mnt/usb/
```

This may take much longer than a move within the same filesystem.

---

## Check Before Moving

Use `ls` before moving wildcard matches:

```bash
ls *.log
```

Then:

```bash
mv *.log logs/
```

---

## Common Options

| Option | Description |
|---|---|
| `-i` | Ask before overwriting |
| `-f` | Force overwrite |
| `-n` | Never overwrite existing files |
| `-v` | Show files being moved |
| `-u` | Move only when source is newer |
| `-b` | Create backup before overwrite |
| `--suffix=SUFFIX` | Set backup suffix |
| `-t DIR` | Specify destination directory |
| `-T` | Treat destination as a normal file/path |
| `--` | End command options |

---

## Common Option Combinations

Interactive and verbose:

```bash
mv -iv file.txt backup/
```

Never overwrite and verbose:

```bash
mv -nv *.txt backup/
```

Update only and verbose:

```bash
mv -uv *.log archive/
```

Backup before overwrite:

```bash
mv -bv file.txt backup/
```

---

## Common Command Patterns

```bash
# Move one file
mv file.txt backup/

# Move multiple files
mv file1.txt file2.txt backup/

# Move all log files
mv *.log logs/

# Rename file
mv old.txt new.txt

# Rename directory
mv old_dir new_dir

# Move directory
mv project/ archive/

# Ask before overwrite
mv -i file.txt backup/

# Never overwrite
mv -n file.txt backup/

# Force overwrite
mv -f file.txt backup/

# Show operation
mv -v file.txt backup/

# Move only if source is newer
mv -u file.txt backup/

# Move filename beginning with "-"
mv -- -file.txt backup/

# Move old logs
find . -type f -name "*.log" -mtime +30 -exec mv -t archive/ {} +
```

## Important Notes

- `mv` is used for both moving and renaming files.
- Moving within the same filesystem is usually very fast.
- Moving between filesystems may internally require copying and deleting.
- Existing destination files may be overwritten unless options such as `-i` or `-n` are used.
- Use `-i` when working with important files.
- Use `-n` when you want to guarantee existing destination files remain untouched.
- Quote paths containing spaces:

```bash
mv "my file.txt" "backup folder/"
```

- Use `--` when a filename begins with `-`.
- Test wildcard matches with `ls` before moving large groups of files.
- GNU-specific options such as `-t`, `-T`, and some backup behavior may differ on macOS/BSD systems.