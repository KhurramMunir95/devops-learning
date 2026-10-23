# Day 02 — Linux `cp` Command

## 1. What is `cp`?

The `cp` command is used to copy files and directories from one location to another.

**Syntax:**

```bash
cp source destination
```

- `source`: File or directory you want to copy.
- `destination`: Location where you want to copy it.

**Example:**

```bash
cp file.txt backup.txt
```

Creates a copy of `file.txt` named `backup.txt` in the current directory.

## 2. Copy a File to Another Directory

```bash
cp errors.txt backup/
```

Copies `errors.txt` into the `backup` directory with the same filename.

## 3. Copy a File with a New Name

```bash
cp errors.txt backup/errors-backup.txt
```

Copies `errors.txt` into the `backup` directory and renames the copy to `errors-backup.txt`.

## 4. Copy a Directory

Use `-r` to copy a directory and its contents recursively.

```bash
cp -r project backup/
```

Copies the `project` directory, including its files and subdirectories, into `backup/`.

## 5. Useful `cp` Options

### `cp -r` — Recursive Copy

Copies a directory and its contents.

```bash
cp -r project backup/
```

### `cp -i` — Interactive Mode

Asks for confirmation before overwriting an existing destination file.

```bash
cp -i file.txt backup/file.txt
```

Type `y` to confirm or `n` to cancel when prompted.

### `cp -v` — Verbose Mode

Displays the source and destination of the copy operation.

```bash
cp -v file.txt backup/
```

Example output:

```text
'file.txt' -> 'backup/file.txt'
```

### `cp -n` — No Clobber

Prevents overwriting an existing destination file.

```bash
cp -n file.txt backup/file.txt
```

If the destination file already exists, its content remains unchanged.

## 6. Copy Files from Different Locations

You do not need to be inside the source or destination directory. You can use **absolute paths** to specify both locations.

**Example:**

```bash
cp /mnt/e/devops-learning/README.md /mnt/e/folder/copy-README.md
```

- Source: `/mnt/e/devops-learning/README.md`
- Destination: `/mnt/e/folder/copy-README.md`

This copies `README.md` into `/mnt/e/folder/` and names the copy `copy-README.md`.

**Important:** The destination directory must already exist. If it does not, create it first:

```bash
mkdir -p /mnt/e/folder
```

## 7. Copy a File or Directory While You Are in Another Location

Suppose your current directory is:

```bash
/home/khurram/devops-learning/week-01-linux
```

But you want to copy a file from `/mnt/e/devops-learning/` to `/mnt/e/backup/`.

You can run:

```bash
cp /mnt/e/devops-learning/README.md /mnt/e/backup/
```

You can copy a directory in the same way:

```bash
cp -r /mnt/e/devops-learning/project /mnt/e/backup/
```

**Key concept:** Your current working directory does not matter when you provide absolute paths.

## 8. Copy Multiple Files

```bash
cp errors.txt access.log backup/
```

Copies both files into the `backup/` directory.

## 9. Quick Reference

| Command | Purpose |
|---|---|
| `cp file.txt backup/` | Copy a file into a directory |
| `cp file.txt backup/new.txt` | Copy a file with a new name |
| `cp -r project backup/` | Copy a directory and its contents |
| `cp -i file.txt backup/` | Ask before overwriting |
| `cp -v file.txt backup/` | Show copy details |
| `cp -n file.txt backup/` | Do not overwrite an existing file |
| `cp source destination` | Copy using relative or absolute paths |

## 10. Practice Tasks

1. Create a directory named `backup`.
2. Copy `README.md` into it.
3. Copy the same file with the name `README-copy.md`.
4. Copy a directory using `cp -r`.
5. Try `cp -v` and observe the output.
6. Try `cp -i` when the destination file already exists.
7. From another working directory, copy a file using its absolute source and destination paths.

## Key Takeaway

The `cp` command copies files and directories. Use `-r` for directories, `-i` for overwrite confirmation, `-v` to see copy details, and `-n` to avoid overwriting existing files.

Using absolute paths lets you copy files between locations without changing your current directory.

# Linux `mv` Command

## 1. Purpose

The `mv` (move) command is used to:

- Move files and directories from one location to another.
- Rename files and directories.
- Move multiple files into a destination directory.

**Syntax:**

```bash
mv [OPTIONS] SOURCE DESTINATION
```

- `SOURCE`: File or directory to move or rename.
- `DESTINATION`: Target location or new name.
- `OPTIONS`: Flags that modify the command's behavior.

## 2. Common Flags

| Flag | Purpose |
|---|---|
| `-i` | Ask for confirmation before overwriting. |
| `-f` | Force the operation without prompting for overwrite confirmation. |
| `-n` | Do not overwrite an existing destination file. |
| `-v` | Display details of the operation. |
| `-u` | Move or overwrite only when the source is newer or the destination is missing. |
| `-t` | Specify the target directory before the source files. |
| `-T` | Treat the destination as a normal target, not as a directory. |

**Note:** Avoid `-f` when you are unsure about overwriting important files.

## 3. Practical Examples

### A. Rename a file

```bash
mv old.txt new.txt
```

Renames `old.txt` to `new.txt` in the same directory.

### B. Move a file into a directory

```bash
mv errors.txt backup/
```

Moves `errors.txt` into the `backup` directory.

### C. Ask before overwriting

```bash
mv -i file.txt backup/
```

If a file with the same name exists in the destination, `mv` asks for confirmation before overwriting it.

### D. Display operation details

```bash
mv -v errors.txt backup/
```

Displays information about the move operation.

### E. Move multiple files

```bash
mv file1.txt file2.txt file3.txt backup/
```

Moves all three files into the `backup` directory.

### F. Prevent overwriting

```bash
mv -n file.txt backup/
```

Does not overwrite an existing file with the same name in `backup/`.

### G. Move only if the source is newer

```bash
mv -u file.txt backup/
```

Moves the file if the destination is missing or the source is newer, subject to the system's file-comparison behavior.

When you run mv -u budget.xlsx Destination/, the computer acts like a smart assistant and looks at the "Last Modified" date and time (timestamp) of both files before doing anything:
• Scenario A (Source is newer): Your source file was saved at 5:00 PM, but the one in the destination folder was saved at 2:00 PM. The assistant sees your source file is newer, so it moves it and overwrites the old one.
• Scenario B (Destination is newer): Your source file was saved at 2:00 PM, but the one in the destination folder was saved at 5:00 PM. The assistant realizes you would lose your latest updates if it moved the old file over it. So, it does nothing and safely skips it.
• Scenario C (File doesn't exist yet): There is no budget.xlsx in the destination folder at all. The assistant just moves it normally.

### H. Specify the target directory first

```bash
mv -t backup/ file1.txt file2.txt
```

Moves both files into `backup/`.

### I. Treat the destination as a file target

```bash
mv -T old.txt new.txt
```

Treats `new.txt` as the exact destination path rather than interpreting it as a directory.

## 4. Move a Directory

```bash
mv project/ backup/
```

Moves the `project` directory into `backup/`.

## 5. Practice Exercise

Create a practice directory and perform the following tasks:

```bash
mkdir -p ~/mv-practice/backup
cd ~/mv-practice

touch test.txt old.txt
mv -v test.txt backup/
mv old.txt new.txt

ls -l
ls -l backup/
```

**Expected results:**

- `test.txt` is inside `backup/`.
- `old.txt` is renamed to `new.txt`.
- `new.txt` remains in the current directory.

## 6. Important Notes

- `mv` moves or renames; it does not normally create an additional copy.
- Use `pwd` to check your current directory.
- Use `ls` to verify file locations.
- Use `mv -i` when you want protection against accidental overwrites.
- A move across different filesystems may require copying and deleting internally.

## 7. Quick Revision

| Task | Command |
|---|---|
| Rename a file | `mv old.txt new.txt` |
| Move a file | `mv file.txt backup/` |
| Show operation details | `mv -v file.txt backup/` |
| Confirm before overwrite | `mv -i file.txt backup/` |
| Prevent overwrite | `mv -n file.txt backup/` |
| Move multiple files | `mv a.txt b.txt backup/` |

**Key takeaway:** Use `mv` to change a file's location or name. Choose flags such as `-i`, `-n`, and `-v` according to the behavior you need.

# Linux `rm` Command

## 1. What is the `rm` Command?

The `rm` command stands for **remove**. It is used to delete files and directories in Linux.

**Syntax:**

```bash
rm [OPTIONS] FILE
```

## 2. Common `rm` Flags

| Flag | Description |
|---|---|
| `-i` | Ask for confirmation before each removal |
| `-f` | Force removal; do not prompt for confirmation or report missing files |
| `-r` | Remove directories and their contents recursively |
| `-R` | Same as `-r` |
| `-d` | Remove empty directories |
| `-v` | Display details about removed files |
| `-I` | Ask for confirmation once before removing multiple files or performing recursive removal |

## 3. Basic Examples

### 3.1 Delete a File

```bash
rm file.txt
```

Deletes `file.txt`.

### 3.2 Delete Multiple Files

```bash
rm file1.txt file2.txt file3.txt
```

Deletes all three files.

### 3.3 Delete a File with Confirmation (`-i`)

```bash
rm -i file.txt
```

Prompts for confirmation before deleting the file.

Example:

```text
rm: remove regular file 'file.txt'?
```

- `y`: Confirm deletion.
- `n`: Cancel deletion.

### 3.4 Force Delete (`-f`)

```bash
rm -f file.txt
```

- Does not prompt for confirmation.
- Does not report an error if the file does not exist.
- Does not bypass file permissions or other system restrictions.

### 3.5 Delete a Directory Recursively (`-r`)

```bash
rm -r folder1
```

Deletes `folder1`, including its files and subdirectories.

Example directory structure:

```text
folder1/
├── file1.txt
├── file2.txt
└── subfolder/
    └── file3.txt
```

After running `rm -r folder1`, the entire directory structure is removed.

### 3.6 Force Delete a Directory (`-rf`)

```bash
rm -rf folder1
```

- `-r`: Recursively removes the directory and its contents.
- `-f`: Suppresses confirmation prompts and errors for missing files.

**Warning:** This command can permanently remove large amounts of data. Always verify the target path before executing it.

### 3.7 Display Removed Files (`-v`)

```bash
rm -v file.txt
```

Displays the name of the file being removed.

You can also combine it with other flags:

```bash
rm -rv folder1
```

### 3.8 Delete an Empty Directory (`-d`)

```bash
rm -d empty_folder
```

Removes the directory only if it is empty.

If the directory contains files or subdirectories, the command fails.

### 3.9 Confirmation for Multiple Removals (`-I`)

```bash
rm -I *.txt
```

Requests confirmation before removing multiple matching files.

The `*.txt` pattern matches files ending in `.txt` in the current directory.

## 4. Combining Flags

Multiple flags can be combined in one command.

```bash
rm -riv folder1
```

- `-r`: Recursively remove the directory and its contents.
- `-i`: Ask for confirmation before each removal.
- `-v`: Display removal details.

Use `-i` when you want to review each deletion.

## 5. Using `rm` with Paths

### Delete a File Using an Absolute Path

```bash
rm /mnt/e/linux/test1.txt
```

Deletes `test1.txt` from the specified location.

### Delete a File Using a Relative Path

```bash
rm folder1/test1.txt
```

Deletes the file relative to the current working directory.

Check your current directory:

```bash
pwd
```

## 6. Delete Files Using Wildcards

### Delete All `.txt` Files in the Current Directory

```bash
rm *.txt
```

### Delete Files Starting with `test`

```bash
rm test*
```

**Caution:** Wildcards can match more files than expected. Use `ls` first to inspect the matching files.

```bash
ls *.txt
```

## 7. `rm` vs `rmdir`

| Command | Purpose |
|---|---|
| `rm file.txt` | Removes a file |
| `rm -r folder1` | Removes a directory and its contents |
| `rm -d empty_folder` | Removes an empty directory |
| `rmdir empty_folder` | Removes an empty directory |

`rmdir` cannot remove a non-empty directory.

## 8. Important Safety Rules

1. `rm` does not normally move files to the Trash; deleted files may be difficult or impossible to recover.
2. Always verify the filename and path before using `rm`.
3. Use `rm -i` when you need confirmation.
4. Be especially careful with `rm -rf`.
5. Never run a destructive command with an unverified variable or path.
6. Avoid running `rm` with `sudo` unless elevated permissions are genuinely necessary.

## 9. Practice Exercises

Create a practice directory:

```bash
mkdir -p ~/rm-practice/subfolder
touch ~/rm-practice/file1.txt
touch ~/rm-practice/subfolder/file2.txt
```

### Exercise 1: Inspect the Directory

```bash
ls -R ~/rm-practice
```

### Exercise 2: Remove a File with Verbose Output

```bash
rm -v ~/rm-practice/file1.txt
```

### Exercise 3: Remove a File with Confirmation

```bash
rm -i ~/rm-practice/subfolder/file2.txt
```

### Exercise 4: Try Removing a Non-Empty Directory

```bash
rmdir ~/rm-practice
```

Observe why it fails: the directory is not empty.

### Exercise 5: Remove the Remaining Directory

```bash
rm -r ~/rm-practice
```

Verify the result:

```bash
ls ~/rm-practice
```

The command should report that the directory does not exist.

## 10. Quick Revision

- `rm`: Remove files.
- `rm -i`: Ask before removing.
- `rm -f`: Force removal.
- `rm -r`: Remove directories recursively.
- `rm -rf`: Force recursive removal.
- `rm -v`: Show removal details.
- `rm -d`: Remove an empty directory.
- `rm -I`: Ask once before a potentially large removal.
- `rmdir`: Remove an empty directory only.

**Remember:** `rm` deletes, `mv` moves or renames, and `cp` copies.

# Linux `cat` Command

## 1. Overview

The `cat` command stands for **concatenate**. It is used to display file contents, combine multiple files, create files, and append content to existing files.

### Syntax

```bash
cat [OPTIONS] [FILE...]
```

## 2. Common Options (Flags)

| Flag | Description |
|---|---|
| `-n` | Number all lines, including blank lines |
| `-b` | Number only non-empty lines |
| `-s` | Squeeze consecutive blank lines into one |
| `-A` | Display non-printing characters, tabs, and line endings visibly |
| `-E` | Display `$` at the end of each line |
| `-T` | Display tab characters as `^I` |
| `-v` | Display most non-printing characters visibly |
| `--help` | Display help information |
| `--version` | Display version information |

## 3. Basic Usage

### 3.1 Display a File

```bash
cat file.txt
```

Displays the contents of `file.txt` in the terminal.

### 3.2 Display Multiple Files

```bash
cat file1.txt file2.txt
```

Displays the contents of both files sequentially.

### 3.3 Display Line Numbers

```bash
cat -n file.txt
```

Numbers all lines, including blank lines.

### 3.4 Number Only Non-Empty Lines

```bash
cat -b file.txt
```

Numbers lines containing text while leaving blank lines unnumbered.

### 3.5 Squeeze Blank Lines

```bash
cat -s file.txt
```

Displays consecutive blank lines as a single blank line. It does not modify the original file.

## 4. Creating and Modifying Files

### 4.1 Create a File

```bash
cat > notes.txt
```

Type your content and press `Ctrl + D` to finish.

**Important:** If `notes.txt` already exists, its previous contents will be overwritten.

### 4.2 Append Content to a File

```bash
cat >> notes.txt
```

Adds new content to the end of the file without removing existing content. Press `Ctrl + D` to finish.

### 4.3 Combine Multiple Files

```bash
cat file1.txt file2.txt > combined.txt
```

Combines the contents of both input files into `combined.txt`.

**Important:** The `>` operator overwrites the destination file if it already exists.

## 5. Inspecting Special Characters

### 5.1 Display Line Endings

```bash
cat -E file.txt
```

Displays `$` at the end of each line, making line boundaries easier to inspect.

### 5.2 Display Tab Characters

```bash
cat -T file.txt
```

Displays tab characters as `^I`.

### 5.3 Display Non-Printing Characters

```bash
cat -v file.txt
```

Makes most non-printing characters visible.

### 5.4 Display All Common Whitespace Markers

```bash
cat -A file.txt
```

Displays tabs, line endings, and other supported non-printing characters visibly.

These options are useful when troubleshooting formatting problems in configuration files and text data.

## 6. Combining Options

Multiple options can be used together.

### Number All Lines and Show Line Endings

```bash
cat -n -E file.txt
```

### Number Non-Empty Lines and Squeeze Blank Lines

```bash
cat -b -s file.txt
```

## 7. Using `cat` with Pipes

A pipe (`|`) sends the output of one command to another command.

### Search for Errors

```bash
cat app.log | grep "ERROR"
```

Displays lines containing `ERROR`.

A more efficient alternative is:

```bash
grep "ERROR" app.log
```

This avoids using `cat` unnecessarily.

### Count Lines

```bash
cat file.txt | wc -l
```

A simpler alternative is:

```bash
wc -l file.txt
```

## 8. `cat` vs Other Commands

| Command | Main Purpose |
|---|---|
| `cat` | Display or concatenate file contents |
| `less` | Read large files page by page |
| `head` | Display the first lines of a file |
| `tail` | Display the last lines of a file |
| `tail -f` | Follow new log entries in real time |
| `grep` | Search for matching text |
| `wc` | Count lines, words, and bytes |

## 9. Practical Exercises

Run the following commands in your Linux terminal.

### Exercise 1: Create a Practice File

```bash
printf 'Linux\n\nDocker\nDevOps\n' > practice.txt
```

### Exercise 2: Display the File

```bash
cat practice.txt
```

### Exercise 3: Display All Line Numbers

```bash
cat -n practice.txt
```

### Exercise 4: Number Non-Empty Lines

```bash
cat -b practice.txt
```

### Exercise 5: Inspect Line Endings

```bash
cat -E practice.txt
```

### Exercise 6: Append New Content

```bash
echo "Nginx" >> practice.txt
```

### Exercise 7: Verify the Changes

```bash
cat practice.txt
```

## 10. Key Takeaways

- `cat file.txt` displays file contents.
- `cat -n file.txt` numbers all lines.
- `cat -b file.txt` numbers only non-empty lines.
- `cat > file.txt` creates or overwrites a file.
- `cat >> file.txt` appends content.
- `cat file1 file2 > combined.txt` combines files.
- `cat -A file.txt` helps inspect special characters.
- Use `less` for large files and `tail -f` for live logs.
- Use `>` carefully because it can overwrite existing data.

**Practice goal:** Be able to explain the common flags, create and append files safely, and inspect text files from the terminal.

# Linux `less` Command

## 1. What Is the `less` Command?

The `less` command is used to view the contents of a file one screen at a time. It is especially useful for reading large files, application logs, and configuration files.

Unlike `cat`, `less` allows you to navigate forward and backward through file content without modifying the file.

## 2. Basic Syntax

```bash
less [OPTIONS] FILE
```

**Example:**

```bash
less application.log
```

This opens `application.log` in the `less` viewer.

## 3. Common `less` Flags

| Flag | Description | Example |
|---|---|---|
| `-N` | Display line numbers | `less -N file.txt` |
| `-S` | Prevent long lines from wrapping | `less -S file.txt` |
| `-i` | Ignore case unless the search pattern contains uppercase letters | `less -i file.txt` |
| `-I` | Ignore case in all searches | `less -I file.txt` |
| `-F` | Exit automatically if the content fits on one screen | `less -F file.txt` |
| `-R` | Display supported ANSI color codes | `less -R file.txt` |
| `-X` | Do not clear the terminal screen when exiting | `less -X file.txt` |
| `-M` | Display a detailed status prompt | `less -M file.txt` |
| `-m` | Display a brief status prompt | `less -m file.txt` |
| `-E` | Exit when reaching the end of the file | `less -E file.txt` |
| `-e` | Exit when reaching the end a second time | `less -e file.txt` |
| `-n` | Suppress line numbers in the status display | `less -n file.txt` |
| `-s` | Collapse consecutive empty lines into one | `less -s file.txt` |
| `-p PATTERN` | Start at the first occurrence of a pattern | `less -p ERROR file.txt` |

## 4. Combining Flags

Multiple flags can be combined in a single command.

```bash
less -N -S application.log
```

This displays line numbers and prevents long lines from wrapping.

The following command is equivalent:

```bash
less -NS application.log
```

**More examples:**

```bash
less -Ni application.log
less -RN application.log
less -XN file.txt
```

## 5. Navigation Keyboard Shortcuts

These commands are used after opening a file in `less`.

| Key | Description |
|---|---|
| `Space` | Move forward one screen |
| `b` | Move backward one screen |
| `Enter` | Move down one line |
| `j` | Move down one line |
| `k` | Move up one line |
| `d` | Move forward half a screen |
| `u` | Move backward half a screen |
| `g` | Go to the beginning of the file |
| `G` | Go to the end of the file |
| `/pattern` | Search forward |
| `?pattern` | Search backward |
| `n` | Go to the next search match |
| `N` | Go to the previous search match |
| `h` | Display help |
| `q` | Quit `less` |

## 6. Useful Examples

### View a File

```bash
less file.txt
```

### View a File with Line Numbers

```bash
less -N file.txt
```

### View Long Lines Without Wrapping

```bash
less -S application.log
```

Use the left and right arrow keys to scroll horizontally.

### Open a File at the End

```bash
less +G application.log
```

### Search for Errors

```bash
less -p ERROR application.log
```

### Read Command Output Page by Page

```bash
ls -la /etc | less
```

### View Matching Log Entries

```bash
grep "ERROR" application.log | less
```

### Display Colored Output

```bash
ls --color=always | less -R
```

The `-R` flag allows supported color escape sequences to be displayed correctly.

## 7. Difference Between `cat`, `more`, and `less`

| Feature | `cat` | `more` | `less` |
|---|---|---|---|
| Displays file content | Yes | Yes | Yes |
| Page-by-page navigation | No | Yes | Yes |
| Scroll backward | No | Limited | Yes |
| Interactive searching | No | Basic | Yes |
| Best use | Small files | Basic pagination | Large files and logs |

## 8. Practical DevOps Use Cases

- **Log analysis:** Read application and system logs.
- **Troubleshooting:** Search for `ERROR`, `WARNING`, or other patterns.
- **Configuration review:** Inspect Nginx and other service configuration files.
- **Large output:** Navigate through lengthy command output without flooding the terminal.
- **Production debugging:** Inspect log files without modifying them.

Example:

```bash
less -N /var/log/nginx/error.log
```

This opens the Nginx error log with line numbers.

## 9. Practice Tasks

1. Open a text file using `less`.
2. Display the file with line numbers using `-N`.
3. Prevent long lines from wrapping using `-S`.
4. Search for `ERROR` using `/ERROR`.
5. Navigate to the end using `G`.
6. Return to the beginning using `g`.
7. Exit using `q`.

## 10. Quick Revision

- `less`: View file content page by page.
- `-N`: Display line numbers.
- `-S`: Prevent long-line wrapping.
- `-i` / `-I`: Configure case-insensitive searching.
- `-R`: Display supported color codes.
- `-F`: Exit if content fits on one screen.
- `-p`: Start at a matching pattern.
- `/pattern`: Search forward.
- `n`: Next match.
- `q`: Quit.

**Best practice:** Use `less` for large log files, `grep` to filter matching lines, and `less -N` when line numbers are useful for debugging.

## 11. Getting Help

```bash
less --help
man less
```

Use `man less` to read the manual for your installed version.


# Linux `head` Command

## 1. Introduction

The `head` command displays the beginning of a file.

- By default, it displays the first **10 lines**.
- It can display a specific number of lines or bytes.
- It is useful for inspecting log files, CSV files, and configuration files.

## 2. Syntax

```bash
head [OPTIONS] [FILE]
```

**Example:**

```bash
head app.log
```

Displays the first 10 lines of `app.log`.

## 3. Important Flags

| Flag | Description |
|---|---|
| `-n NUM` | Display the first NUM lines. |
| `-c NUM` | Display the first NUM bytes. |
| `-n -NUM` | Display all lines except the last NUM lines. |
| `-c -NUM` | Display all bytes except the last NUM bytes. |
| `-q` | Suppress file-name headers when displaying multiple files. |
| `-v` | Always display file-name headers. |
| `-z` | Use NUL characters as line delimiters (GNU `head`). |
| `--help` | Display help information (GNU `head`). |
| `--version` | Display version information (GNU `head`). |

## 4. Basic Examples

### 4.1 Display the First 10 Lines

```bash
head app.log
```

Displays the first 10 lines by default.

### 4.2 Display the First 5 Lines

```bash
head -n 5 app.log
```

Displays only the first 5 lines.

### 4.3 Display the First Line

```bash
head -n 1 app.log
```

Displays only the first line.

### 4.4 Display the First 20 Lines

```bash
head -n 20 app.log
```

Displays the first 20 lines.

### 4.5 Use the Short Form

```bash
head -5 app.log
```

Equivalent to `head -n 5 app.log`.

### 4.6 Display the First 20 Bytes

```bash
head -c 20 app.log
```

Displays the first 20 bytes, not 20 lines or necessarily 20 characters.

## 5. Negative Values

### 5.1 Exclude the Last 2 Lines

```bash
head -n -2 app.log
```

Displays all lines except the last 2.

### 5.2 Exclude the Last 10 Bytes

```bash
head -c -10 app.log
```

Displays all bytes except the last 10.

**Note:** Negative values are supported by GNU `head` and may not work on every Unix implementation.

## 6. Working with Multiple Files

### 6.1 Display Multiple Files

```bash
head -n 3 app.log error.log
```

Displays the first 3 lines of each file, with file-name headers.

### 6.2 Suppress File-Name Headers

```bash
head -q -n 3 app.log error.log
```

Displays the first 3 lines from each file without file-name headers.

### 6.3 Always Display File-Name Headers

```bash
head -v app.log
```

Displays the file-name header even when reading a single file.

## 7. Using `head` with Pipes

### 7.1 Display the First 5 Lines of Command Output

```bash
ls -l | head -n 5
```

Displays the first 5 lines of the directory listing.

### 7.2 Display the First 5 Matching Errors

```bash
grep "ERROR" app.log | head -n 5
```

Finds lines containing `ERROR` and displays the first 5 matches.

### 7.3 Preview CSV Data

```bash
head -n 10 users.csv
```

Displays the header and the first 9 data rows, assuming the CSV contains a header row.

### 7.4 Read from Standard Input

```bash
cat app.log | head -n 5
```

Displays the first 5 lines from standard input.

For a single file, prefer:

```bash
head -n 5 app.log
```

## 8. Practical Linux and DevOps Use Cases

### Inspect a Log File

```bash
head -n 20 /var/log/syslog
```

Quickly inspect the beginning of a system log.

### Preview a Configuration File

```bash
head -n 10 /etc/hosts
```

Inspect the first 10 lines of the hosts file.

### Preview a Large File

```bash
head -n 5 large-file.txt
```

Read a small portion without printing the entire file to the terminal.

### Find the First 3 Errors

```bash
grep "ERROR" app.log | head -n 3
```

Useful for quickly reviewing error messages.

## 9. Difference Between `head`, `tail`, and `less`

| Command | Purpose | Example |
|---|---|---|
| `head` | Display the beginning of a file. | `head -n 10 app.log` |
| `tail` | Display the end of a file. | `tail -n 10 app.log` |
| `less` | Navigate through a file interactively. | `less app.log` |

For continuously monitoring new log entries, use:

```bash
tail -f app.log
```

## 10. Important Points to Remember

1. `head` displays the first 10 lines by default.
2. Use `-n` to control the number of lines.
3. Use `-c` to control the number of bytes.
4. Negative values can exclude content from the end when using GNU `head`.
5. Use `-q` to suppress file-name headers for multiple files.
6. Use `-v` to always display file-name headers.
7. Use pipes to process the output of other Linux commands.
8. `head` is commonly used to inspect logs and large files quickly.

## 11. Practice Exercises

Try these commands using your own files.

1. Display the first 5 lines of `app.log`.
2. Display the first 15 bytes of `app.log`.
3. Display all lines except the last 3 lines.
4. Display the first 3 lines containing `ERROR`.
5. Display the first 5 lines from two files without file-name headers.
6. Display the first 10 lines of `/etc/hosts`.

### Practice Answers

```bash
# 1. First 5 lines
head -n 5 app.log

# 2. First 15 bytes
head -c 15 app.log

# 3. All lines except the last 3
head -n -3 app.log

# 4. First 3 ERROR matches
grep "ERROR" app.log | head -n 3

# 5. First 5 lines from two files, without headers
head -q -n 5 app.log error.log

# 6. First 10 lines of /etc/hosts
head -n 10 /etc/hosts
```

## 12. Quick Revision

| Requirement | Command |
|---|---|
| First 10 lines | `head file.txt` |
| First 5 lines | `head -n 5 file.txt` |
| First line | `head -n 1 file.txt` |
| First 20 bytes | `head -c 20 file.txt` |
| Exclude last 2 lines | `head -n -2 file.txt` |
| First 3 lines of multiple files | `head -n 3 file1.txt file2.txt` |
| Suppress file headers | `head -q file1.txt file2.txt` |
| Show file headers | `head -v file.txt` |
| First 5 matching errors | `grep "ERROR" file.txt \| head -n 5` |

**Final takeaway:** Use `head` when you need to inspect the beginning of a file or limit the output of another command.

# Linux `tail` Command

## 1. What Is `tail`?

The `tail` command displays the last part of a file. By default, it displays the last **10 lines**.

### Syntax

```bash
tail [OPTIONS] [FILE]
```

### Example

```bash
tail app.log
```

Displays the last 10 lines of `app.log`.

---

## 2. Common `tail` Options

| Option | Description | Example |
|---|---|---|
| `-n N` | Display the last N lines | `tail -n 5 app.log` |
| `-n +N` | Display from line N to the end | `tail -n +5 app.log` |
| `-c N` | Display the last N bytes | `tail -c 100 app.log` |
| `-f` | Follow new content added to a file | `tail -f app.log` |
| `-F` | Follow a file by name, including after rotation or recreation | `tail -F app.log` |
| `-q` | Suppress filename headers when reading multiple files | `tail -q file1.txt file2.txt` |
| `-v` | Always display filenames | `tail -v app.log` |
| `-s N` | Set the interval between follow checks in seconds | `tail -f -s 2 app.log` |
| `--pid=PID` | Stop following when the specified process exits | `tail --pid=1234 -f app.log` |
| `--help` | Display help information | `tail --help` |

---

## 3. Display a Specific Number of Lines

### Display the last 5 lines

```bash
tail -n 5 app.log
```

### Display the last 50 lines

```bash
tail -n 50 app.log
```

### Display content from line 5 to the end

```bash
tail -n +5 app.log
```

**Important difference:**

- `tail -n 5 file.txt` — displays the last 5 lines.
- `tail -n +5 file.txt` — displays from line 5 through the end of the file.

---

## 4. Monitor Logs in Real Time

### Follow a log file

```bash
tail -f app.log
```

Displays the last 10 lines and continues showing new lines as they are appended.

Press `Ctrl + C` to stop monitoring.

### Display the last 20 lines and continue monitoring

```bash
tail -n 20 -f app.log
```

### Follow a log after rotation

```bash
tail -F app.log
```

The `-F` option keeps trying to follow the file by name if it is rotated, removed, or recreated.

### Monitor multiple log files

```bash
tail -f app.log error.log
```

Displays new content from both files.

---

## 5. Display Line Numbers with `tail`

The `tail` command does not have a built-in option to number each output line. You can combine it with `nl`.

### Number the last 10 lines

```bash
tail -n 10 app.log | nl
```

This numbers the displayed output starting from 1. It does not preserve the original file line numbers.

### Preserve original line numbers

```bash
nl -ba app.log | tail -n 10
```

- `nl` — numbers lines.
- `-ba` — numbers all lines, including blank lines.
- `tail -n 10` — displays the last 10 numbered lines.

**Note:** `nl -ba app.log | tail -n 10` processes and numbers the entire file before selecting the last 10 lines. For very large files, this can be less efficient.

---

## 6. Remove Empty Lines

The `tail` command does not remove blank lines. Use `sed` or `grep` for this task.

### Display a file without empty lines

```bash
sed '/^$/d' app.log
```

### Also remove lines containing only spaces or tabs

```bash
sed '/^[[:space:]]*$/d' app.log
```

### Remove blank lines and display the last 10 remaining lines

```bash
sed '/^[[:space:]]*$/d' app.log | tail -n 10
```

These commands display the result without changing the original file.

---

## 7. Filter Errors from Logs

### Display the last 50 lines containing `ERROR`

```bash
grep 'ERROR' app.log | tail -n 50
```

### Monitor new errors in real time

```bash
tail -f app.log | grep --line-buffered 'ERROR'
```

### Match errors regardless of letter case

```bash
tail -f app.log | grep --line-buffered -i 'error'
```

### Monitor warnings and errors

```bash
tail -f app.log | grep --line-buffered -Ei 'warning|error'
```

`--line-buffered` helps matching lines appear promptly when `grep` is used in a pipeline.

---

## 8. Display the Last Part of a File in Bytes

### Display the last 100 bytes

```bash
tail -c 100 app.log
```

The `-c` option counts bytes rather than lines. It is useful when inspecting the end of a file at the byte level.

---

## 9. Practical DevOps Examples

### Inspect recent application logs

```bash
tail -n 100 app.log
```

### Monitor an application while reproducing a bug

```bash
tail -f app.log
```

### Monitor errors in production logs

```bash
tail -f app.log | grep --line-buffered -i 'error'
```

### Inspect recent system messages

```bash
tail -n 50 /var/log/syslog
```

The `/var/log/syslog` file is available on some Linux distributions; other systems may use different log files or `journalctl`.

### Monitor systemd service logs

```bash
journalctl -u nginx -f
```

This is an alternative to `tail` when logs are managed by systemd's journal.

---

## 10. `head` vs `tail`

| Feature | `head` | `tail` |
|---|---|---|
| Default output | First 10 lines | Last 10 lines |
| `-n 5` | First 5 lines | Last 5 lines |
| `-n +5` | From line 5 to the end | From line 5 to the end |
| Real-time following | Not supported by standard GNU `head` | Supported with `-f` |
| Common use | Inspect file beginnings | Inspect recent logs |

---

## 11. Practice Exercises

Try these commands in your Ubuntu/WSL terminal.

1. Display the last 15 lines of `app.log`.
2. Display the last 30 lines and monitor new content.
3. Display everything from line 8 to the end.
4. Display the last 10 lines with original line numbers.
5. Remove blank lines and display the last 10 remaining lines.
6. Display the last 50 lines containing `ERROR`.
7. Monitor new log entries containing either `WARNING` or `ERROR`.
8. Follow a log file even if it is recreated.

### Answers

```bash
# 1. Last 15 lines
tail -n 15 app.log

# 2. Last 30 lines and follow new content
tail -n 30 -f app.log

# 3. From line 8 to the end
tail -n +8 app.log

# 4. Last 10 lines with original line numbers
nl -ba app.log | tail -n 10

# 5. Last 10 non-blank lines
sed '/^[[:space:]]*$/d' app.log | tail -n 10

# 6. Last 50 matching error lines
grep 'ERROR' app.log | tail -n 50

# 7. Monitor warnings and errors
tail -f app.log | grep --line-buffered -Ei 'warning|error'

# 8. Follow a recreated or rotated file
tail -F app.log
```

---

## 12. Key Takeaways

- `tail` displays the last 10 lines by default.
- `-n` controls how many lines are displayed.
- `-n +N` displays content starting at line N.
- `-f` monitors new content in real time.
- `-F` is useful when log files are rotated or recreated.
- `nl -ba file | tail -n N` displays the last N lines with original line numbers.
- `sed '/^[[:space:]]*$/d'` removes blank lines from displayed output.
- Combining `tail` with `grep` is useful for troubleshooting application and server logs.

**DevOps habit:** Use `tail -f` or `journalctl -f` to observe logs while reproducing a problem, then filter relevant messages to narrow down the cause.
