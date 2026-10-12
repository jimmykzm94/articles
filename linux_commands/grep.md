# Linux Command Cheatsheet: `grep`

`grep` searches text for matching words or patterns.

## Basic Syntax

```bash
grep [options] pattern file
```

Example:

```bash
grep "error" log.txt
```

---

## Search for Text

Search for a word:

```bash
grep "error" log.txt
```

Search multiple files:

```bash
grep "error" file1.txt file2.txt
```

Search using shell wildcard:

```bash
grep "error" *.log
```

---

## Ignore Case

```bash
grep -i "error" log.txt
```

Matches:

```text
error
Error
ERROR
```

---

## Show Line Numbers

```bash
grep -n "error" log.txt
```

Example output:

```text
25:error connecting to server
```

---

## Recursive Search

Search all files inside a directory:

```bash
grep -r "TODO" src/
```

Show line numbers:

```bash
grep -rn "TODO" src/
```

Ignore case:

```bash
grep -ri "error" logs/
```

---

## Match Whole Words

```bash
grep -w "error" log.txt
```

Matches:

```text
error
```

Does not match:

```text
errors
error_code
```

---

## Match Entire Line

```bash
grep -x "hello world" file.txt
```

The whole line must exactly match.

---

## Invert the Match

Show lines that do **not** contain a pattern:

```bash
grep -v "debug" log.txt
```

Useful for filtering output:

```bash
ps aux | grep -v "grep"
```

---

## Count Matching Lines

```bash
grep -c "error" log.txt
```

Example:

```text
15
```

---

## Show Only Matching Text

```bash
grep -o "error" log.txt
```

Example input:

```text
error: another error occurred
```

Output:

```text
error
error
```

---

## Show Filename Only

Show files containing a match:

```bash
grep -l "error" *.log
```

Show files that do not contain a match:

```bash
grep -L "error" *.log
```

---

## Suppress Filename

When searching multiple files:

```bash
grep -h "error" *.log
```

---

## Always Show Filename

```bash
grep -H "error" log.txt
```

---

## Show Context Around a Match

Show 3 lines after:

```bash
grep -A 3 "error" log.txt
```

Show 3 lines before:

```bash
grep -B 3 "error" log.txt
```

Show 3 lines before and after:

```bash
grep -C 3 "error" log.txt
```

---

## Extended Regular Expressions

Use `-E`:

```bash
grep -E "error|warning" log.txt
```

Match multiple alternatives:

```bash
grep -E "failed|error|timeout" log.txt
```

Match numbers:

```bash
grep -E "[0-9]+" file.txt
```

---

## Match Beginning of Line

```bash
grep "^ERROR"