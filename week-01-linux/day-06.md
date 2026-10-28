# Day 06: Searching Text with `grep`

## 1. Purpose

The `grep` command is used to search for specific words, text, or patterns inside files or command output.

## 2. Basic Syntax

```bash
grep [flags] "search_text" filename
```

## 3. Important Flags

| Flag | Purpose |
|---|---|
| `-i` | Search without case sensitivity |
| `-n` | Display matching line numbers |
| `-v` | Display non-matching lines |
| `-r` | Search recursively inside directories |
| `-l` | Display filenames containing matches |
| `-c` | Count matching lines |
| `-w` | Match complete words only |
| `-o` | Display only the matching text |
| `-E` | Use extended regular expressions |

## 4. Hands-on Practice

### Step 1: Create a practice directory and log file

```bash
mkdir -p ~/devops-lab/day6
cd ~/devops-lab/day6
nano app.log
```

Add the following sample logs:

```text
INFO Server started
ERROR Database connection failed
ERROR Port 3000 already in use
WARNING High memory usage
ERROR Permission denied
ERROR Connection timed out
ERROR Out of memory
ERROR Invalid API key
INFO Server stopped
```

Save the file using `Ctrl + O`, press `Enter`, and exit with `Ctrl + X`.

### Step 2: Practice searching

**1. Search for errors**

```bash
grep "ERROR" app.log
```

**2. Search without case sensitivity**

```bash
grep -i "error" app.log
```

**3. Display line numbers**

```bash
grep -n "ERROR" app.log
```

**4. Count matching error lines**

```bash
grep -c "ERROR" app.log
```

**5. Display lines that do not contain errors**

```bash
grep -v "ERROR" app.log
```

**6. Search for multiple patterns**

```bash
grep -E "ERROR|WARNING" app.log
```

**7. Display only the matching text**

```bash
grep -o "ERROR" app.log
```

## 5. Important Notes

- Linux commands and filenames are generally case-sensitive.
- The `-i` flag ignores case sensitivity.
- The `-c` flag counts matching lines, not individual occurrences.
- Multiple flags can be combined, for example, `grep -in "error" app.log`.
- Exit status `0` means a match was found, `1` means no match was found, and `2` indicates an error.

## 6. Day 06 Outcome

- [ ] Search for text using `grep`.
- [ ] Use the `-i`, `-n`, `-v`, and `-c` flags.
- [ ] Search for multiple patterns.
- [ ] Identify errors in log files.

# Linux `find` Command

## 1. Goal

Learn how to search for files and directories by name, type, size, modification time, and other conditions. Also learn how to combine `find` with `grep`.

## 2. Basic Syntax

```bash
find [path] [options] [conditions]
```

- `.` — current directory
- `/mnt/e/` — Windows E: drive in WSL
- `-type f` — regular files
- `-type d` — directories

## 3. Important Options

| Option | Purpose |
|---|---|
| `-name` | Search by filename (case-sensitive) |
| `-iname` | Search by filename (case-insensitive) |
| `-type f` | Find files only |
| `-type d` | Find directories only |
| `-empty` | Find empty files or directories |
| `-size` | Search by file size |
| `-mtime` | Search by modification time in days |
| `-mmin` | Search by modification time in minutes |
| `-maxdepth` | Limit search depth |
| `-user` | Search by file owner |
| `-perm` | Search by permissions |
| `-exec` | Execute a command on matching results |
| `-print` | Print matching paths |

## 4. Search by Filename

### Exact filename

```bash
find . -type f -name "app.log"
```

### Filename without knowing the extension

```bash
find . -type f -name "day-01*"
```

Matches `day-01.md`, `day-01.txt`, and `day-01-notes`.

### Filename ends with a specific pattern

```bash
find . -type f -name "*day-01"
```

Matches `backup-day-01`, but not `backup-day-01.txt`.

### Pattern anywhere in the filename

```bash
find . -type f -name "*day-01*"
```

### Case-insensitive search

```bash
find . -type f -iname "*DAY-01*"
```

### Search for a directory

```bash
find . -type d -name "devops-test"
```

### Search a Windows drive in WSL

```bash
find /mnt/e/ -type d -name "devops-test" 2>/dev/null
```

`2>/dev/null` hides permission errors. It does not grant additional permissions.

## 5. Search by Other Conditions

### Find empty files

```bash
find . -type f -empty
```

### Find files larger than 10 MB

```bash
find . -type f -size +10M
```

### Find files modified within approximately the last 24 hours

```bash
find . -type f -mmin -1440
```

### Find files modified more than 7 days ago

```bash
find . -type f -mtime +7
```

### Limit search depth

```bash
find . -maxdepth 2 -type f
```

## 6. Combine `find` with `grep`

Use `find` to locate files and `grep` to search their contents.

### Search for errors in a specific file

```bash
find . -type f -name "app.log" -exec grep -i "error" {} \;
```

### Include line numbers

```bash
find . -type f -name "app.log" -exec grep -in "error" {} \;
```

### Search all log files for errors

```bash
find . -type f -name "*.log" -exec grep -in "error" {} +
```

### Search using a pipe

```bash
find . -type f -name "app.log" -print0 | xargs -0 grep -in "error"
```

**Important syntax:**

- `{}` represents each matching file.
- `\;` ends the `-exec` command for each file.
- `+` allows multiple matching files to be passed to one command invocation.
- `-print0` and `xargs -0` safely handle filenames containing spaces or special characters.

## 7. Practical Exercises

Create a practice directory:

```bash
mkdir -p ~/devops-lab/find-practice
cd ~/devops-lab/find-practice

touch app.log error.log day-01.md day-02.md
mkdir devops-test
touch empty.txt
```

Practice these commands:

1. Find all `.log` files.
2. Find filenames containing `day`.
3. Find all directories.
4. Find empty files.
5. Find files modified in the last 24 hours.
6. Search all `.log` files for `error`, ignoring case.
7. Find the directory named `devops-test`.

## 8. Important Notes

- `find` searches for files and directories; `grep` searches text inside files.
- `-name "day-01"` requires an exact filename match.
- `-name "day-01*"` matches names starting with `day-01`.
- `-name "*day-01"` matches names ending with `day-01`.
- `-name "*day-01*"` matches names containing `day-01`.
- Use `-iname` when case should not matter.
- Permission errors may mean some paths cannot be searched.

## 9. Outcome

- [ ] Search for files and directories by name.
- [ ] Use wildcards and case-insensitive matching.
- [ ] Filter results by type, size, and modification time.
- [ ] Combine `find` and `grep` to search log files.
- [ ] Understand and troubleshoot permission errors.