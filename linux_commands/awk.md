# Linux Command Cheatsheet: `awk`

`awk` is a text-processing tool used to filter, extract, transform, and summarize structured text.

It is especially useful for column-based data.

## Basic Syntax

```bash
awk 'pattern { action }' file
```

Example:

```bash
awk '{ print $1 }' file.txt
```

Prints the first field of every line.

---

## Field Variables

`awk` automatically splits each line into fields.

| Variable | Meaning |
|---|---|
| `$0` | Entire line |
| `$1` | First field |
| `$2` | Second field |
| `$3` | Third field |
| `$NF` | Last field |
| `NF` | Number of fields |
| `NR` | Current record/line number |
| `FNR` | Current line number within current file |

Example input:

```text
Alice 25 Engineer
Bob 31 Manager
```

Print first column:

```bash
awk '{ print $1 }' file.txt
```

Output:

```text
Alice
Bob
```

---

## Print Multiple Fields

```bash
awk '{ print $1, $3 }' file.txt
```

Output:

```text
Alice Engineer
Bob Manager
```

---

## Print the Entire Line

```bash
awk '{ print $0 }' file.txt
```

Equivalent to:

```bash
awk '{ print }' file.txt
```

---

## Print the Last Field

```bash
awk '{ print $NF }' file.txt
```

Useful when the number of columns varies.

---

## Print Second Last Field

```bash
awk '{ print $(NF-1) }' file.txt
```

---

## Set the Input Delimiter

Use `-F`.

For CSV:

```bash
awk -F',' '{ print $1 }' file.csv
```

For colon-separated files:

```bash
awk -F':' '{ print $1 }' /etc/passwd
```

---

## Multiple-Character Field Separator

Using a regular expression:

```bash
awk -F'[,:]' '{ print $1, $2 }' file.txt
```

Splits using either `,` or `:`.

---

## Set Output Field Separator

Use `OFS`:

```bash
awk 'BEGIN { OFS="," } { print $1, $2 }' file.txt
```

Output:

```text
Alice,25
Bob,31
```

---

## Print Line Numbers

```bash
awk '{ print NR, $0 }' file.txt
```

Example:

```text
1 Alice 25 Engineer
2 Bob 31 Manager
```

---

## Print Number of Fields

```bash
awk '{ print NF }' file.txt
```

Print line number and number of fields:

```bash
awk '{ print NR, NF }' file.txt
```

---

## Filter by Column Value

Print rows where column 2 is greater than 30:

```bash
awk '$2 > 30 { print }' file.txt
```

Print selected fields:

```bash
awk '$2 > 30 { print $1, $2 }' file.txt
```

---

## Compare Strings

```bash
awk '$3 == "Engineer" { print $1 }' file.txt
```

Not equal:

```bash
awk '$3 != "Engineer" { print }' file.txt
```

---

## Search for a Pattern

```bash
awk '/error/ { print }' log.txt
```

Equivalent to a simple `grep`-style search.

Case-sensitive by default.

---

## Search and Print Specific Fields

```bash
awk '/error/ { print $1, $2 }' log.txt
```

---

## Negate a Pattern

```bash
awk '!/debug/ { print }' log.txt
```

Shows lines that do not contain `debug`.

---

## Multiple Conditions

AND:

```bash
awk '$2 > 20 && $3 == "Engineer" { print }' file.txt
```

OR:

```bash
awk '$2 > 30 || $3 == "Manager" { print }' file.txt
```

NOT:

```bash
awk '$3 != "Manager" { print }' file.txt
```

---

## Numeric Comparison

```bash
awk '$2 > 100 { print }' file.txt
```

Operators:

```text
==
!=
>
<
>=
<=
```

---

## Regular Expression Matching

Match:

```bash
awk '$1 ~ /^A/ { print }' file.txt
```

Print rows where field 1 starts with `A`.

Does not match:

```bash
awk '$1 !~ /^A/ { print }' file.txt
```

---

## Match Beginning of Line

```bash
awk '/^ERROR/ { print }' log.txt
```

---

## Match End of Line

```bash
awk '/failed$/ { print }' log.txt
```

---

## BEGIN Block

`BEGIN` runs before input is processed.

```bash
awk 'BEGIN { print "Starting..." } { print }' file.txt
```

Useful for setting variables:

```bash
awk 'BEGIN { FS="," } { print $1 }' file.csv
```

---

## END Block

`END` runs after all input is processed.

Count total lines:

```bash
awk 'END { print NR }' file.txt
```

---

## BEGIN + Main + END

```bash
awk '
BEGIN {
    print "Report"
}
{
    print $1, $2
}
END {
    print "Total:", NR
}
' file.txt
```

---

## Sum a Column

Sum column 2:

```bash
awk '{ sum += $2 } END { print sum }' file.txt
```

---

## Calculate Average

```bash
awk '{ sum += $2 } END { print sum / NR }' file.txt
```

Safer version:

```bash
awk '{ sum += $2 } END { if (NR > 0) print sum / NR }' file.txt
```

---

## Find Maximum Value

```bash
awk 'NR == 1 || $2 > max { max = $2 } END { print max }' file.txt
```

---

## Find Minimum Value

```bash
awk 'NR == 1 || $2 < min { min = $2 } END { print min }' file.txt
```

---

## Count Matching Lines

```bash
awk '/error/ { count++ } END { print count }' log.txt
```

---

## Count Values by Category

Example input:

```text
Alice Engineer
Bob Manager
Charlie Engineer
```

Command:

```bash
awk '{ count[$2]++ } END { for (x in count) print x, count[x] }' file.txt
```

Possible output:

```text
Engineer 2
Manager 1
```

---

## Print a Range of Lines

Lines 5 through 10:

```bash
awk 'NR >= 5 && NR <= 10' file.txt
```

Only line 5:

```bash
awk 'NR == 5' file.txt
```

---

## Print Odd Lines

```bash
awk 'NR % 2 == 1' file.txt
```

---

## Print Even Lines

```bash
awk 'NR % 2 == 0' file.txt
```

---

## Skip First Line

Useful for CSV headers:

```bash
awk 'NR > 1 { print }' file.csv
```

---

## Process Header Separately

```bash
awk '
NR == 1 {
    print "HEADER:", $0
    next
}
{
    print $1
}
' file.txt
```

`next` skips the rest of the rules for the current line.

---

## Set Variables from the Command Line

Use `-v`:

```bash
awk -v limit=100 '$2 > limit { print }' file.txt
```

Multiple variables:

```bash
awk -v min=10 -v max=100 '$2 >= min && $2 <= max' file.txt
```

---

## Use Shell Variables

```bash
limit=50
awk -v value="$limit" '$2 > value { print }' file.txt
```

Prefer `-v` instead of directly inserting shell variables into the `awk` program.

---

## Change a Field

```bash
awk '{$2 = 100; print}' file.txt
```

Example:

```text
Alice 25 Engineer
```

becomes:

```text
Alice 100 Engineer
```

---

## Replace Text

Use `sub()`:

```bash
awk '{ sub(/old/, "new"); print }' file.txt
```

Replaces the first match on each line.

---

## Replace All Matches

Use `gsub()`:

```bash
awk '{ gsub(/old/, "new"); print }' file.txt
```

---

## Replace Only in One Field

```bash
awk '{ gsub(/old/, "new", $2); print }' file.txt
```

---

## Remove Leading Whitespace

```bash
awk '{ sub(/^[[:space:]]+/, ""); print }' file.txt
```

---

## Remove Trailing Whitespace

```bash
awk '{ sub(/[[:space:]]+$/, ""); print }' file.txt
```

---

## Remove Both Leading and Trailing Whitespace

```bash
awk '{
    gsub(/^[[:space:]]+|[[:space:]]+$/, "")
    print
}' file.txt
```

---

## Change Output Format

Use `printf`.

```bash
awk '{ printf "%s -> %s\n", $1, $2 }' file.txt
```

Example output:

```text
Alice -> 25
Bob -> 31
```

---

## Format Numbers

Two decimal places:

```bash
awk '{ printf "%.2f\n", $2 }' file.txt
```

Aligned columns:

```bash
awk '{ printf "%-20s %10d\n", $1, $2 }' file.txt
```

---

## `print` vs `printf`

`print` automatically adds a newline:

```bash
awk '{ print $1 }' file.txt
```

`printf` gives more formatting control:

```bash
awk '{ printf "%s ", $1 }' file.txt
```

---

## String Length

```bash
awk '{ print length($0) }' file.txt
```

Length of first field:

```bash
awk '{ print length($1) }' file.txt
```

---

## Extract Part of a String

Use `substr()`:

```bash
awk '{ print substr($1, 1, 3) }' file.txt
```

Starts at character 1 and returns 3 characters.

---

## Convert to Uppercase

```bash
awk '{ print toupper($0) }' file.txt
```

Specific field:

```bash
awk '{ print toupper($1) }' file.txt
```

---

## Convert to Lowercase

```bash
awk '{ print tolower($0) }' file.txt
```

---

## Split a String

```bash
awk '{
    split($1, parts, "-")
    print parts[1], parts[2]
}' file.txt
```

Example input:

```text
2026-10
```

Output:

```text
2026 10
```

---

## Conditional Logic

```bash
awk '{
    if ($2 >= 50)
        print $1, "PASS"
    else
        print $1, "FAIL"
}' file.txt
```

---

## Ternary Operator

```bash
awk '{ print $1, ($2 >= 50 ? "PASS" : "FAIL") }' file.txt
```

---

## Arrays

Count occurrences:

```bash
awk '{ count[$1]++ } END { for (x in count) print x, count[x] }' file.txt
```

Arrays in `awk` are associative arrays.

---

## Delete Array Element

```bash
awk '{
    data[$1] = $2
    delete data[$1]
}'
```

---

## Multiple Input Files

```bash
awk '{ print }' file1.txt file2.txt
```

`NR` continues across files.

`FNR` restarts for each file.

Example:

```bash
awk '{ print NR, FNR, $0 }' file1.txt file2.txt
```

---

## Show Current Filename

```bash
awk '{ print FILENAME, $0 }' file.txt
```

Useful when processing multiple files:

```bash
awk '/error/ { print FILENAME, NR, $0 }' *.log
```

---

## Process Different Files Differently

```bash
awk '
FILENAME == "file1.txt" { print "A:", $0 }
FILENAME == "file2.txt" { print "B:", $0 }
' file1.txt file2.txt
```

---

## Read from Standard Input

```bash
echo "Alice 25 Engineer" | awk '{ print $1 }'
```

With another command:

```bash
ps aux | awk '{ print $1, $2, $11 }'
```

---

## Combine with `grep`

```bash
grep "error" log.txt | awk '{ print $1, $2 }'
```

Often `awk` can do both:

```bash
awk '/error/ { print $1, $2 }' log.txt
```

---

## Combine with `sort`

```bash
awk '{ print $2 }' file.txt | sort -n
```

---

## Combine with `uniq`

```bash
awk '{ print $1 }' file.txt | sort | uniq
```

Count occurrences:

```bash
awk '{ print $1 }' file.txt | sort | uniq -c
```

---

## Combine with `ps`

Show process ID and command:

```bash
ps aux | awk '{ print $2, $11 }'
```

Filter high CPU usage:

```bash
ps aux | awk '$3 > 50 { print $2, $3, $11 }'
```

---

## Combine with `df`

Show filesystem and usage:

```bash
df -h | awk '{ print $1, $5 }'
```

Find filesystems above 80% usage:

```bash
df -P | awk 'NR > 1 {
    gsub(/%/, "", $5)
    if ($5 > 80)
        print $1, $5 "%"
}'
```

---

## Combine with `free`

```bash
free -m | awk '/Mem:/ { print $3 " MB used" }'
```

---

## Combine with `ip`

Extract IPv4 addresses:

```bash
ip -4 addr | awk '/inet / { print $2 }'
```

---

## Combine with `/etc/passwd`

Print usernames:

```bash
awk -F':' '{ print $1 }' /etc/passwd
```

Print username and shell:

```bash
awk -F':' '{ print $1, $7 }' /etc/passwd
```

---

## CSV Example

Input:

```text
name,score,status
Alice,85,pass
Bob,45,fail
Charlie,92,pass
```

Print users who passed:

```bash
awk -F',' 'NR > 1 && $3 == "pass" { print $1, $2 }' file.csv
```

---

## Use an AWK Script File

Create:

```text
script.awk
```

Contents:

```awk
BEGIN {
    print "Report"
}

{
    print $1, $2
}

END {
    print "Total:", NR
}
```

Run:

```bash
awk -f script.awk file.txt
```

---

## Built-in Variables

| Variable | Description |
|---|---|
| `NR` | Total current record number |
| `FNR` | Record number in current file |
| `NF` | Number of fields |
| `FS` | Input field separator |
| `OFS` | Output field separator |
| `RS` | Input record separator |
| `ORS` | Output record separator |
| `FILENAME` | Current filename |

---

## Change Field Separator with `FS`

```bash
awk 'BEGIN { FS=":" } { print $1 }' /etc/passwd
```

Equivalent to:

```bash
awk -F':' '{ print $1 }' /etc/passwd
```

---

## Change Record Separator

Normally, one line is one record.

Change it with `RS`:

```bash
awk 'BEGIN { RS=";" } { print }' file.txt
```

Now `;` separates records.

---

## Change Output Record Separator

```bash
awk 'BEGIN { ORS=" | " } { print $1 }' file.txt
```

---

## Exit Early

```bash
awk '/error/ {
    print
    exit
}' log.txt
```

Stops after the first matching line.

---

## Skip Current Line

Use `next`:

```bash
awk '$1 == "debug" { next } { print }' file.txt
```

Equivalent idea:

```text
skip lines beginning with debug
```

---

## Common Functions

| Function | Description |
|---|---|
| `length()` | String length |
| `substr()` | Extract substring |
| `split()` | Split string into array |
| `sub()` | Replace first match |
| `gsub()` | Replace all matches |
| `tolower()` | Convert to lowercase |
| `toupper()` | Convert to uppercase |
| `index()` | Find substring position |
| `sprintf()` | Format and return string |
| `int()` | Convert to integer |

---

## Common Command Patterns

```bash
# Print first field
awk '{ print $1 }' file.txt

# Print first and third fields
awk '{ print $1, $3 }' file.txt

# CSV first column
awk -F',' '{ print $1 }' file.csv

# Print last field
awk '{ print $NF }' file.txt

# Line numbers
awk '{ print NR, $0 }' file.txt

# Filter numeric value
awk '$2 > 100 { print }' file.txt

# Filter text
awk '$3 == "ERROR" { print }' file.txt

# Pattern search
awk '/error/ { print }' log.txt

# Sum second column
awk '{ sum += $2 } END { print sum }' file.txt

# Average second column
awk '{ sum += $2 } END { print sum / NR }' file.txt

# Count lines
awk 'END { print NR }' file.txt

# Skip header
awk 'NR > 1' file.csv

# Lines 10 to 20
awk 'NR >= 10 && NR <= 20' file.txt

# Replace text
awk '{ gsub(/old/, "new"); print }' file.txt

# Print usernames from /etc/passwd
awk -F':' '{ print $1 }' /etc/passwd
```

## Important Notes

- `awk` works naturally with rows and columns.
- Fields are separated by whitespace by default.
- Use `-F` or `FS` to change the input delimiter.
- `$0` represents the entire line.
- `$1`, `$2`, and so on represent individual fields.
- `$NF` is useful for accessing the last field.
- `NR` counts records across all input files, while `FNR` resets for each file.
- `BEGIN` runs before input processing and `END` runs afterward.
- `awk` supports variables, arrays, loops, conditions, arithmetic, and regular expressions.
- For simple filtering, `grep` may be shorter; for field extraction and calculations, `awk` is usually more powerful.