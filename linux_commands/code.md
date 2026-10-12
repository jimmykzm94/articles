# Linux Command Cheatsheet: `code`

`code` launches and controls **Visual Studio Code** from the terminal.

## Basic Syntax

```bash
code [options] [file | directory]
```

Open VS Code:

```bash
code
```

---

## Open Current Directory

```bash
code .
```

One of the most commonly used commands.

---

## Open a Directory

```bash
code project/
```

Absolute path:

```bash
code /home/user/project/
```

---

## Open a File

```bash
code main.c
```

Open multiple files:

```bash
code main.c main.h
```

If the file does not exist, VS Code opens it as a new file:

```bash
code new_file.c
```

---

## Open a New Window

```bash
code -n .
```

Or:

```bash
code --new-window .
```

---

## Reuse Existing Window

```bash
code -r .
```

Or:

```bash
code --reuse-window .
```

Useful when you do not want another VS Code window.

---

## Open at Specific Line

```bash
code -g main.c:100
```

Open at line and column:

```bash
code -g main.c:100:20
```

Long form:

```bash
code --goto main.c:100:20
```

---

## Compare Two Files

Open the VS Code diff editor:

```bash
code -d old.c new.c
```

Or:

```bash
code --diff old.c new.c
```

Useful for comparing:

```bash
code --diff config.old config.new
```

---

## Merge Files

Three-way merge:

```bash
code --merge modified1 modified2 base result
```

Structure:

```text
modified1
modified2
base
result
```

Useful for Git merge conflicts.

---

## Wait Until File Is Closed

```bash
code -w file.txt
```

Or:

```bash
code --wait file.txt
```

The terminal waits until the file is closed in VS Code.

Useful with Git:

```bash
git config --global core.editor "code --wait"
```

Then commands such as:

```bash
git commit
```

can use VS Code as the editor.

---

## Open Multiple Directories

```bash
code project1/ project2/
```

VS Code opens them as a multi-root workspace.

---

## Add Directory to Current Workspace

```bash
code --add library/
```

---

## Remove Directory from Workspace

```bash
code --remove library/
```

---

## Open with a Profile

```bash
code . --profile "C Development"
```

Example:

```bash
code ~/projects/firmware --profile "Embedded"
```

If the profile does not exist, VS Code can create a new empty profile.

---

# Extensions

## List Installed Extensions

```bash
code --list-extensions
```

---

## Install an Extension

```bash
code --install-extension publisher.extension
```

Example:

```bash
code --install-extension ms-vscode.cpptools
```

---

## Install from VSIX

```bash
code --install-extension extension.vsix
```

---

## Uninstall an Extension

```bash
code --uninstall-extension publisher.extension
```

Example:

```bash
code --uninstall-extension ms-vscode.cpptools
```

---

## Show Extension Versions

```bash
code --list-extensions --show-versions
```

Example output:

```text
ms-vscode.cpptools@1.x.x
ms-python.python@20xx.x.x
```

---

## Disable All Extensions

```bash
code --disable-extensions
```

Useful when debugging VS Code problems.

---

## Disable One Extension

```bash
code --disable-extension publisher.extension
```

---

# VS Code Information

## Show Version

```bash
code --version
```

Short form:

```bash
code -v
```

---

## Show Help

```bash
code --help
```

Short form:

```bash
code -h
```

---

## Show Process Status

```bash
code --status
```

Useful for checking:

- CPU usage
- memory usage
- VS Code processes

---

## Verbose Output

```bash
code --verbose
```

Useful for troubleshooting startup problems.

---

## Disable GPU Acceleration

```bash
code --disable-gpu
```

Useful when troubleshooting graphical or rendering problems.

---

# User Data and Extensions

## Use Different User Data Directory

```bash
code --user-data-dir /tmp/vscode-test
```

Useful for testing VS Code with isolated:

- settings
- state
- configuration

---

## Use Different Extension Directory

```bash
code --extensions-dir /path/to/extensions
```

---

# Read from Standard Input

Use `-` to read data from stdin:

```bash
echo "Hello Linux" | code -
```

Another example:

```bash
cat log.txt | code -
```

---

# Change Language

```bash
code --locale en-US
```

Example:

```bash
code --locale zh-TW
```

---

# Remote Development

Connect to a remote environment:

```bash
code --remote <authority> <path>
```

For example, remote features can be used with environments such as:

```text
SSH
WSL
Remote development
```

---

# VS Code Tunnel

Start a remote tunnel:

```bash
code tunnel
```

Show tunnel options:

```bash
code tunnel --help
```

---

# Start VS Code Chat from Terminal

Recent VS Code versions support the `chat` subcommand:

```bash
code chat "Explain this project"
```

Use the current directory as project context:

```bash
cd project/
code chat "Explain how this project works"
```

Specify chat mode:

```bash
code chat --mode ask "Explain main.c"
```

```bash
code chat --mode edit "Fix this implementation"
```

```bash
code chat --mode agent "Find and fix the problem"
```

Add a file as context:

```bash
code chat --add-file main.c "Explain this code"
```

Pipe command output into chat:

```bash
make 2>&1 | code chat "Why did the build fail?" -
```

---

# VS Code Insiders

VS Code Insiders uses:

```bash
code-insiders .
```

Instead of:

```bash
code .
```

---

# Linux / macOS PATH

Check whether `code` is available:

```bash
which code
```

Example:

```text
/usr/bin/code
```

If the shell returns:

```text
code: command not found
```

VS Code is not available through your `PATH`.

On macOS, open the VS Code Command Palette and run:

```text
Shell Command: Install 'code' command in PATH
```

Then restart the terminal.

---

# Common Options

| Option | Description |
|---|---|
| `.` | Open current directory |
| `-n`, `--new-window` | Open new window |
| `-r`, `--reuse-window` | Reuse existing window |
| `-g`, `--goto` | Open at line/column |
| `-d`, `--diff` | Compare two files |
| `-m`, `--merge` | Three-way file merge |
| `-w`, `--wait` | Wait until files are closed |
| `--add` | Add folder to workspace |
| `--remove` | Remove folder from workspace |
| `--profile` | Use a VS Code profile |
| `--install-extension` | Install extension |
| `--uninstall-extension` | Uninstall extension |
| `--list-extensions` | List extensions |
| `--disable-extensions` | Disable all extensions |
| `--locale` | Set display language |
| `--status` | Show process information |
| `--verbose` | Show verbose diagnostic output |
| `--disable-gpu` | Disable GPU acceleration |
| `-v`, `--version` | Show version |
| `-h`, `--help` | Show CLI help |

---

# Common Command Patterns

```bash
# Open current directory
code .

# Open file
code main.c

# Open project
code ~/projects/firmware/

# Open new window
code -n .

# Reuse existing window
code -r .

# Open at line 100
code -g main.c:100

# Open at line 100, column 20
code -g main.c:100:20

# Compare files
code -d old.c new.c

# Wait until file closes
code -w file.txt

# List extensions
code --list-extensions

# Install extension
code --install-extension ms-vscode.cpptools

# Show version
code --version

# Troubleshoot without extensions
code --disable-extensions

# Open with specific profile
code . --profile "Embedded"

# CLI help
code --help
```

## Important Notes

- `code .` opens the current directory and is probably the most commonly used `code` command.
- `-r` is useful when you want to reuse your existing VS Code window.
- `-g` is useful when scripts or compiler output provides a specific line number.
- `--diff` provides a convenient graphical file comparison.
- `--wait` is useful when configuring VS Code as the editor for Git or other command-line tools.
- Extensions can be installed, removed, and listed without opening the Extensions UI.
- On macOS, you may need to manually install the `code` command into `PATH`.
- VS Code Insiders uses `code-insiders` instead of `code`.