# Linux Command Cheatsheet: `tar`

`tar` archives multiple files and directories into one file. It can also compress archives using gzip, bzip2, or xz.

## Basic Syntax

```bash
tar [options] archive-name files-or-directories
```

## Main Operations

| Option | Description |
|---|---|
| `-c` | Create an archive |
| `-x` | Extract an archive |
| `-t` | List archive contents |
| `-r` | Append files to an archive |
| `-u` | Add files newer than archived copies |
| `-f` | Specify the archive filename |
| `-v` | Show processed files |

Only use one main operation at a time: `-c`, `-x`, `-t`, `-r`, or `-u`.

## Create an Archive

```bash
tar -cf archive.tar file1.txt file2.txt
```

Archive a directory:

```bash
tar -cf project.tar project/
```

Show files while archiving:

```bash
tar -cvf project.tar project/
```

## Extract an Archive

```bash
tar -xf archive.tar
```

Extract with verbose output:

```bash
tar -xvf archive.tar
```

Extract into another directory:

```bash
tar -xf archive.tar -C /path/to/destination
```

The destination directory must already exist.

## List Archive Contents

```bash
tar -tf archive.tar
```

Detailed listing:

```bash
tar -tvf archive.tar
```

## Compression Options

| Option | Compression | Common extension |
|---|---|---|
| `-z` | gzip | `.tar.gz`, `.tgz` |
| `-j` | bzip2 | `.tar.bz2` |
| `-J` | xz | `.tar.xz` |

## Create Compressed Archives

Create a gzip archive:

```bash
tar -czf archive.tar.gz directory/
```

Create a bzip2 archive:

```bash
tar -cjf archive.tar.bz2 directory/
```

Create an xz archive:

```bash
tar -cJf archive.tar.xz directory/
```

With verbose output:

```bash
tar -czvf archive.tar.gz directory/
```

## Extract Compressed Archives

Extract gzip:

```bash
tar -xzf archive.tar.gz
```

Extract bzip2:

```bash
tar -xjf archive.tar.bz2
```

Extract xz:

```bash
tar -xJf archive.tar.xz
```

Modern versions of `tar` can often detect compression automatically:

```bash
tar -xf archive.tar.gz
```

## Extract Specific Files

```bash
tar -xf archive.tar path/to/file.txt
```

Extract multiple files:

```bash
tar -xf archive.tar file1.txt file2.txt
```

The file path must match the path stored inside the archive.

## Extract Matching Files

```bash
tar -xf archive.tar --wildcards "*.log"
```

Extract files from a specific directory:

```bash
tar -xf archive.tar --wildcards "logs/*"
```

## Exclude Files

Exclude one file:

```bash
tar -czf archive.tar.gz directory/ --exclude="directory/file.log"
```

Exclude a pattern:

```bash
tar -czf archive.tar.gz directory/ --exclude="*.log"
```

Exclude multiple patterns:

```bash
tar -czf archive.tar.gz directory/ \
  --exclude="*.log" \
  --exclude="node_modules" \
  --exclude=".git"
```

## Remove Leading Directories

Remove one directory level during extraction:

```bash
tar -xf archive.tar --strip-components=1
```

Example stored path:

```text
project/src/main.c
```

After `--strip-components=1`:

```text
src/main.c
```

## Preserve Permissions

Preserve file permissions during extraction:

```bash
tar -xpf archive.tar
```

This is often most relevant when running as `root`.

## Keep Existing Files

Do not overwrite existing files:

```bash
tar -xf archive.tar --keep-old-files
```

Skip existing files without reporting an error:

```bash
tar -xf archive.tar --skip-old-files
```

## Overwrite Existing Files

```bash
tar -xf archive.tar --overwrite
```

Use carefully because existing files may be replaced.

## Append Files

Append a file to an uncompressed archive:

```bash
tar -rf archive.tar new-file.txt
```

Append a directory:

```bash
tar -rf archive.tar new-directory/
```

Appending directly to compressed archives is generally not supported.

## Update an Archive

Add files only when they are newer than the archived versions:

```bash
tar -uf archive.tar directory/
```

## Delete from an Archive

```bash
tar --delete -f archive.tar path/to/file.txt
```

This generally works only with uncompressed archives.

## Read Files from Standard Input

Archive filenames listed in a text file:

```bash
tar -cf archive.tar -T files.txt
```

Example `files.txt`:

```text
file1.txt
file2.txt
documents/
```

## Write an Archive to Standard Output

```bash
tar -czf - directory/ > archive.tar.gz
```

Send an archive through SSH:

```bash
tar -czf - directory/ | ssh user@server "cat > archive.tar.gz"
```

Archive a remote directory locally:

```bash
ssh user@server "tar -czf - /path/to/directory" > backup.tar.gz
```

## Change Directory Before Archiving

```bash
tar -czf archive.tar.gz -C /var/www project/
```

This archives:

```text
project/
```

Instead of storing the full path:

```text
/var/www/project/
```

## Common Backup Command

```bash
tar -czvf backup-$(date +%Y-%m-%d).tar.gz \
  --exclude="*.log" \
  --exclude="cache" \
  /path/to/data
```

## Inspect Before Extracting

Always inspect unknown archives first:

```bash
tar -tf archive.tar.gz
```

Then extract:

```bash
tar -xf archive.tar.gz
```

## Common Command Patterns

```bash
# Create
tar -cvf archive.tar directory/

# Create with gzip
tar -czvf archive.tar.gz directory/

# Extract
tar -xvf archive.tar

# Extract compressed archive
tar -xzvf archive.tar.gz

# List contents
tar -tvf archive.tar

# Extract somewhere else
tar -xf archive.tar -C /destination/

# Exclude files
tar -czf archive.tar.gz directory/ --exclude="*.log"

# Remove the first path level
tar -xf archive.tar --strip-components=1
```

## Option Order

These commands are equivalent:

```bash
tar -czvf archive.tar.gz directory/
```

```bash
tar czvf archive.tar.gz directory/
```

Using the leading hyphen is clearer and more consistent with other Linux commands.

## Important Notes

- `tar` combines files; compression is handled by gzip, bzip2, or xz.
- Use `-f` immediately before the archive filename.
- Use `-C` to control where files are archived from or extracted to.
- Inspect unfamiliar archives before extracting them.
- Avoid extracting untrusted archives as `root`.
- Be careful with absolute paths and files containing `../`.