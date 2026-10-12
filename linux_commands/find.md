# Linux Command Cheatsheet: `find`

`find` searches for files and directories based on name, type, size, time, permissions, and other conditions.

## Basic Syntax

```bash
find [path] [conditions] [actions]
```

Example:

```bash
find . -name "file.txt"
```

`.` means the current directory.

---

## Find by Name

Find an exact filename:

```bash
find . -name "config.txt"
```

Find using wildcards:

```bash
find . -name "*.log"
```

Case-insensitive search:

```bash
find . -iname "*.jpg"
```

Multiple filename patterns:

```bash
find . \( -name "*.c" -o -name "*.h" \)
```

---

## Find Files Only

```bash
find . -type f
```

Find only `.c` files:

```bash
find . -type f -name "*.c"
```

---

## Find Directories Only

```bash
find . -type d
```

Find a directory by name:

```bash
find . -type d -name "build"
```

Common types:

| Option | Description |
|---|---|
| `-type f` | Regular file |
| `-type d` | Directory |
| `-type l` | Symbolic link |
| `-type b` | Block device |
| `-type c` | Character device |
| `-type p` | Named pipe |
| `-type s` | Socket |

---

## Search from a Specific Directory

```bash
find /var/log -name "*.log"
```

Search the home directory:

```bash
find ~ -name "*.conf"
```

Search multiple locations:

```bash
find /etc /usr/local/etc -name "*.conf"
```

---

## Limit Search Depth

Search only the current directory:

```bash
find . -maxdepth 1 -type f
```

Search up to 2 directory levels:

```bash
find . -maxdepth 2 -type f
```

Ignore the first directory level:

```bash
find . -mindepth 2 -type f
```

Combine both:

```bash
find . -mindepth 1 -maxdepth 2 -type f
```

---

## Find by File Size

Files larger than 100 MB:

```bash
find . -type f -size +100M
```

Files smaller than 10 KB:

```bash
find . -type f -size -10k
```

Files around 1 GB:

```bash
find . -type f -size 1G
```

Common size units:

| Unit | Meaning |
|---|---|
| `c` | Bytes |
| `k` | KiB |
| `M` | MiB |
| `G` | GiB |

`+` means greater than.

`-` means less than.

---

## Find Recently Modified Files

Modified within the last 7 days:

```bash
find . -type f -mtime -7
```

Modified more than 30 days ago:

```bash
find . -type f -mtime +30
```

Modified exactly around 7 days ago:

```bash
find . -type f -mtime 7
```

---

## Find by Minutes

Modified within the last 30 minutes:

```bash
find . -type f -mmin -30
```

Modified more than 60 minutes ago:

```bash
find . -type f -mmin +60
```

---

## Time Options

| Option | Description |
|---|---|
| `-mtime` | Content modification time in days |
| `-mmin` | Content modification time in minutes |
| `-ctime` | Metadata/status change time in days |
| `-cmin` | Metadata/status change time in minutes |
| `-atime` | Last access time in days |
| `-amin` | Last access time in minutes |

---

## Find Newer Than Another File

```bash
find . -type f -newer reference.txt
```

Useful for finding files modified after a build or timestamp file.

---

## Find by Date

GNU `find`:

```bash
find . -type f -newermt "2026-10-01"
```

Between two dates:

```bash
find . -type f \
  -newermt "2026-10-01" \
  ! -newermt "2026-10-04"
```

---

## Find Empty Files

```bash
find . -type f -empty
```

Find empty directories:

```bash
find . -type d -empty
```

---

## Find by Owner

```bash
find . -user username
```

Find by group:

```bash
find . -group developers
```

Find files not owned by a specific user:

```bash
find . ! -user username
```

---

## Find by Permissions

Exact permission:

```bash
find . -type f -perm 644
```

Files where all specified bits are set:

```bash
find . -type f -perm -644
```

Files writable by anyone:

```bash
find . -type f -perm -002
```

Executable files:

```bash
find . -type f -executable
```

---

## Find Symbolic Links

```bash
find . -type l
```

Find broken symbolic links:

```bash
find . -xtype l
```

Follow symbolic links while searching:

```bash
find -L . -name "*.conf"
```

Use carefully because symbolic-link loops may exist.

---

## Exclude a Directory

Skip `.git`:

```bash
find . -path "./.git" -prune -o -type f -print
```

Skip `build`:

```bash
find . -path "./build" -prune -o -type f -print
```

Skip multiple directories:

```bash
find . \
  \( -path "./.git" -o -path "./build" \) \
  -prune -o \
  -type f -print
```

---

## Exclude a File Pattern

Find everything except `.log` files:

```bash
find . -type f ! -name "*.log"
```

Exclude multiple extensions:

```bash
find . -type f \
  ! -name "*.log" \
  ! -name "*.tmp"
```

---

## AND Conditions

Conditions are ANDed by default:

```bash
find . -type f -name "*.log" -size +10M
```

Means:

```text
Regular file
AND
Name ends in .log
AND
Larger than 10 MB
```

---

## OR Conditions

Use `-o`:

```bash
find . \( -name "*.c" -o -name "*.h" \)
```

Another example:

```bash
find . -type f \( -name "*.jpg" -o -name "*.png" \)
```

Parentheses must normally be escaped from the shell.

---

## NOT Conditions

```bash
find . ! -name "*.log"
```

Or:

```bash
find . -not -name "*.log"
```

Example:

```bash
find . -type f ! -name "*.tmp"
```

---

## Print Results

Default:

```bash
find . -name "*.txt"
```

Explicit:

```bash
find . -name "*.txt" -print
```

Print full details:

```bash
find . -type f -ls
```

---

## Custom Output

GNU `find` supports `-printf`:

```bash
find . -type f -printf "%p\n"
```

Show filename and size:

```bash
find . -type f -printf "%p %s bytes\n"
```

Show modification date:

```bash
find . -type f -printf "%TY-%Tm-%Td %TH:%TM %p\n"
```

Useful format fields:

| Format | Description |
|---|---|
| `%p` | Full path |
| `%f` | Filename |
| `%s` | Size in bytes |
| `%u` | Owner |
| `%g` | Group |
| `%m` | Permissions |
| `%TY` | Modification year |
| `%Tm` | Modification month |
| `%Td` | Modification day |

---

## Execute a Command on Results

Run `ls -l` for each result:

```bash
find . -type f -name "*.log" -exec ls -l {} \;
```

`{}` represents the matched file.

`\;` ends the command.

---

## Execute in Batches

More efficient:

```bash
find . -type f -name "*.log" -exec ls -l {} +
```

Instead of running:

```text
ls file1
ls file2
ls file3
```

It can run something similar to:

```text
ls file1 file2 file3
```

---

## Delete Matching Files

```bash
find . -type f -name "*.tmp" -delete
```

Before deleting, test first:

```bash
find . -type f -name "*.tmp"
```

Then:

```bash
find . -type f -name "*.tmp" -delete
```

---

## Delete Empty Directories

```bash
find . -type d -empty -delete
```

---

## Delete Old Log Files

Delete `.log` files older than 30 days:

```bash
find /var/log -type f -name "*.log" -mtime +30 -delete
```

Test without `-delete` first.

---

## Change Permissions

Set `.sh` files executable:

```bash
find . -type f -name "*.sh" -exec chmod +x {} +
```

Set directory permissions:

```bash
find . -type d -exec chmod 755 {} +
```

Set file permissions:

```bash
find . -type f -exec chmod 644 {} +
```

---

## Find and Search File Contents

Combine `find` with `grep`:

```bash
find . -type f -name "*.c" -exec grep -n "TODO" {} +
```

Or commonly:

```bash
grep -rn --include="*.c" "TODO" .
```

---

## Find the Largest Files

```bash
find . -type f -printf "%s %p\n" |
  sort -nr |
  head
```

Human-readable alternative:

```bash
find . -type f -exec du -h {} + |
  sort -h |
  tail
```

---

## Find Files Modified Today

GNU `find`:

```bash
find . -type f -newermt "today"
```

---

## Find Files Modified in the Last Hour

```bash
find . -type f -mmin -60
```

---

## Find Files Larger Than 1 GB

```bash
find / -type f -size +1G 2>/dev/null
```

`2>/dev/null` hides permission-denied errors.

---

## Find Executable Files

```bash
find . -type f -executable
```

Find executable shell scripts:

```bash
find . -type f -name "*.sh" -executable
```

---

## Find Hidden Files

```bash
find . -type f -name ".*"
```

Find hidden directories:

```bash
find . -type d -name ".*"
```

---

## Find Specific File Extensions

```bash
find . -type f \( \
  -name "*.c" \
  -o -name "*.cpp" \
  -o -name "*.h" \
\)
```

---

## Common Options

| Option | Description |
|---|---|
| `-name` | Match filename |
| `-iname` | Case-insensitive filename |
| `-type` | Match file type |
| `-size` | Match file size |
| `-mtime` | Modification time in days |
| `-mmin` | Modification time in minutes |
| `-atime` | Access time |
| `-ctime` | Metadata change time |
| `-user` | Match owner |
| `-group` | Match group |
| `-perm` | Match permissions |
| `-empty` | Match empty files/directories |
| `-newer` | Newer than reference file |
| `-maxdepth` | Maximum search depth |
| `-mindepth` | Minimum search depth |
| `-exec` | Execute a command |
| `-delete` | Delete matches |
| `-print` | Print matches |
| `-prune` | Skip a directory |
| `-o` | OR |
| `!` | NOT |

---

## Common Command Patterns

```bash
# Find file by name
find . -name "config.txt"

# Ignore case
find . -iname "config.txt"

# Find all C files
find . -type f -name "*.c"

# Find directory
find . -type d -name "build"

# Files larger than 100 MB
find . -type f -size +100M

# Modified within 24 hours
find . -type f -mtime -1

# Modified within 30 minutes
find . -type f -mmin -30

# Empty files
find . -type f -empty

# Exclude log files
find . -type f ! -name "*.log"

# C or C++ files
find . -type f \( -name "*.c" -o -name "*.cpp" \)

# Skip .git directory
find . -path "./.git" -prune -o -type f -print

# Execute command
find . -name "*.log" -exec ls -lh {} +

# Delete temporary files
find . -type f -name "*.tmp" -delete

# Files larger than 1 GB
find / -type f -size +1G 2>/dev/null
```

## Important Notes

- `find` searches directories recursively by default.
- Use `-type f` when you only want files.
- Use `-type d` when you only want directories.
- Conditions are combined with AND by default.
- Use `-o` for OR and `!` for NOT.
- Quote wildcard patterns such as `"*.log"` so the shell does not expand them first.
- Test a `find` command before adding `-delete` or destructive `-exec` operations.
- `-exec ... {} +` is usually more efficient than `-exec ... {} \;` when the command supports multiple files.
- GNU/Linux and macOS/BSD `find` have some differences, especially options such as `-printf` and `-newermt`.