# Linux Master Challenge --- Production Server Rescue

> **Learning track:** DevOps --- Linux Fundamentals\
> **Environment:** Ubuntu / WSL\
> **Difficulty:** Advanced beginner\
> **Coverage:** Days 1--6\
> **Goal:** Practise Linux filesystem navigation, file operations,
> package management, users and groups, permissions, and log
> investigation through a realistic server-maintenance scenario.

------------------------------------------------------------------------

## 1. Challenge Scenario

You are responsible for a Linux server hosting a production application.
The team has reported:

-   Application errors appearing in log files.
-   Deployment files that need organizing.
-   Backups that need copying and renaming.
-   Incorrect permissions on configuration files.
-   A need to inspect the current Linux user and groups.
-   A need to verify package availability and edit server notes.

The task is to organize the lab environment, investigate the logs,
correct file permissions, and prepare reports.

> **Safety:** Perform all practice inside your own `production-server`
> lab directory. Do not experiment with destructive commands on `/`,
> your home directory, or real project directories. Use `sudo` only when
> required and understand the command before running it.

## 2. Lab Directory Structure

Create this structure inside your home directory:

``` text
production-server/
├── app/
│   ├── frontend/
│   └── backend/
├── logs/
│   ├── archive/
│   └── current/
├── backups/
│   ├── daily/
│   └── weekly/
├── config/
└── reports/
```

### Commands

``` bash
cd ~
mkdir -p production-server/{app/{frontend,backend},logs/{archive,current},backups/{daily,weekly},config,reports}
pwd
ls -R production-server
```

### Navigation practice

``` bash
cd ~/production-server/app/backend
pwd

cd ../../logs/current
pwd

cd ~
pwd
```

**Key concepts**

-   `pwd` prints the current working directory.
-   `ls` lists directory contents; `ls -R` lists recursively.
-   `cd` changes the current directory.
-   `mkdir -p` creates parent directories as needed and does not fail if
    the target directory already exists.
-   `~` represents the current user's home directory.
-   Relative paths are interpreted from the current directory; absolute
    paths start from `/`.

## 3. Deployment File Management

Create sample application and configuration files:

``` bash
cd ~/production-server

touch app/frontend/{index.html,app.js,style.css}
touch app/backend/{server.js,database.js,package.json}
touch config/{production.env,nginx.conf}
touch backups/daily/backup-01.tar
```

Copy and rename files:

``` bash
cp backups/daily/backup-01.tar backups/weekly/
mv backups/daily/backup-01.tar backups/daily/backup-01-old.tar
mv app/frontend/style.css app/frontend/app.css
```

Create and remove a temporary lab file:

``` bash
touch reports/temp.txt
rm reports/temp.txt
```

Verify the result:

``` bash
ls -l backups/daily
ls -l backups/weekly
ls -l app/frontend
ls -l reports
```

**Key concepts**

-   `touch` creates an empty file if it does not exist, or updates its
    timestamp if it does.
-   `cp` copies files; it leaves the original in place.
-   `mv` moves or renames files.
-   `rm` removes files. It does not normally move them to a recycle bin.
-   `cat` prints file contents to the terminal.

## 4. Production Log Investigation

Create `logs/current/app.log` and edit it with Nano:

``` bash
nano logs/current/app.log
```

Paste this sample log data:

``` text
2026-10-30 09:00:01 INFO Server started
2026-10-30 09:01:15 INFO Database connected
2026-10-30 09:02:10 ERROR Database connection timeout
2026-10-30 09:02:15 WARN Retrying database connection
2026-10-30 09:03:20 ERROR Database connection timeout
2026-10-30 09:04:10 INFO User login successful
2026-10-30 09:05:30 ERROR API request failed
2026-10-30 09:06:10 ERROR API request failed
2026-10-30 09:07:15 WARN High memory usage
2026-10-30 09:08:20 INFO Health check passed
2026-10-30 09:09:10 ERROR Permission denied
2026-10-30 09:10:15 INFO Backup completed
2026-10-30 09:11:20 ERROR Permission denied
2026-10-30 09:12:30 WARN High memory usage
2026-10-30 09:13:40 INFO Server running normally
```

### Read and inspect the log

``` bash
cat logs/current/app.log
less logs/current/app.log
head -n 5 logs/current/app.log
tail -n 5 logs/current/app.log
```

In `less`, use arrow keys or Page Up/Page Down to navigate, `/ERROR` to
search, and `q` to quit.

### Search log entries

``` bash
grep "ERROR" logs/current/app.log
grep "WARN" logs/current/app.log
grep -i "error" logs/current/app.log
```

-   `grep` searches for matching text.
-   `-i` makes the search case-insensitive.

### Count lines and save matching entries

``` bash
wc -l logs/current/app.log

grep "ERROR" logs/current/app.log > reports/errors.log
cat reports/errors.log
```

Append another line without overwriting the report:

``` bash
echo "Error investigation completed" >> reports/errors.log
```

**Redirection concepts**

-   `>` writes output to a file and overwrites existing contents.
-   `>>` appends output to the end of a file.
-   `|` sends one command's output to another command as input.

## 5. Bonus Challenge --- Error Frequency and Unique Errors

Run these commands from `~/production-server`.

### 5.1 Count repeated complete error lines

``` bash
grep "ERROR" logs/current/app.log | sort | uniq -c | sort -nr
```

This counts identical full log lines. Because the sample log has
different timestamps, each full line appears only once. This is useful
for understanding why timestamps should be removed when grouping errors
by message.

### 5.2 Count each unique error message

``` bash
grep "ERROR" logs/current/app.log \
  | sed 's/^.*ERROR //' \
  | sort \
  | uniq -c \
  | sort -nr
```

Expected output:

``` text
2 API request failed
2 Database connection timeout
2 Permission denied
```

Explanation:

-   `grep "ERROR"` keeps error entries.
-   `sed 's/^.*ERROR //'` removes the timestamp and `ERROR` prefix.
-   `sort` places identical messages next to each other.
-   `uniq -c` counts each consecutive group of identical lines.
-   `sort -nr` sorts counts numerically, highest first.

### 5.3 Generate a report containing only unique error messages

``` bash
grep "ERROR" logs/current/app.log \
  | sed 's/^.*ERROR //' \
  | sort -u \
  > reports/unique-errors.txt
```

Read the report:

``` bash
cat reports/unique-errors.txt
```

Expected output:

``` text
API request failed
Database connection timeout
Permission denied
```

### 5.4 Save error frequencies to a report

``` bash
grep "ERROR" logs/current/app.log \
  | sed 's/^.*ERROR //' \
  | sort \
  | uniq -c \
  | sort -nr \
  > reports/error-frequency.txt
```

Verify:

``` bash
cat reports/error-frequency.txt
```

**Important:** `uniq` only detects adjacent duplicate lines. Use `sort`
before `uniq` when you need to group duplicates from an unsorted file.
`sort -u` sorts and removes duplicate lines.

## 6. File Permissions and Ownership

Inspect permissions:

``` bash
ls -l config
ls -l config/production.env
```

Set `production.env` so only its owner can read and write it:

``` bash
chmod 600 config/production.env
```

Set `nginx.conf` so the owner can read/write and group and others can
read:

``` bash
chmod 644 config/nginx.conf
```

Give a lab script executable permission:

``` bash
touch reports/check-server.sh
chmod 755 reports/check-server.sh
```

Verify:

``` bash
ls -l config reports/check-server.sh
```

### Understand `rwx`

  Permission   Meaning                       Numeric value
  ------------ --------------------------- ---------------
  `r`          Read                                      4
  `w`          Write                                     2
  `x`          Execute / enter directory                 1
  `-`          Permission not granted                    0

Permission groups appear in this order:

1.  Owner
2.  Group
3.  Others

Examples:

-   `600` = owner read/write; group and others have no permissions.
-   `644` = owner read/write; group and others read-only.
-   `755` = owner read/write/execute; group and others read/execute.

### Change ownership

On a lab file, change the owner to `root`:

``` bash
sudo chown root reports/check-server.sh
ls -l reports/check-server.sh
```

To change both owner and group, the general syntax is:

``` bash
sudo chown USER:GROUP FILE
```

Use a real user and group that exist on your own system.

**Note:** `chown` changes ownership; `chmod` changes permissions. A file
owned by `root` may no longer be editable by your normal user unless
permissions or ownership are changed appropriately.

## 7. Users, Groups, and Sudo

Inspect the current account:

``` bash
whoami
id
groups
```

Run an administrative command:

``` bash
sudo apt update
```

Inspect a lab file's ownership:

``` bash
ls -l reports/check-server.sh
```

**Key concepts**

-   `whoami` displays the effective username.
-   `id` displays user ID (UID), primary group ID (GID), and
    supplementary groups.
-   `groups` displays group memberships.
-   `sudo` runs an authorized command with elevated privileges.
-   Use elevated privileges only when necessary. Do not assume every
    command needs `sudo`.

## 8. Package Management and Nano

Refresh package information:

``` bash
sudo apt update
```

Search for Nano:

``` bash
apt search nano
```

Check whether Nano is installed:

``` bash
nano --version
```

If it is not installed:

``` bash
sudo apt install nano
```

Create and edit server notes:

``` bash
nano reports/server-notes.txt
```

Suggested note contents:

``` text
Current username: [your username]
Server directory: ~/production-server
Application status: Sample lab environment
Errors found: Database connection timeout, API request failed, Permission denied
Backup location: ~/production-server/backups
```

Save in Nano with `Ctrl + O`, press Enter to confirm, and exit with
`Ctrl + X`.

**Package-management concepts**

-   `sudo apt update` refreshes the local package index; it does not
    upgrade installed packages.
-   `apt search` searches package names and descriptions.
-   `sudo apt install` installs a package.
-   Nano is a terminal text editor.

## 9. Final Investigation --- Combined Commands

Run these from the lab root:

``` bash
cd ~/production-server
```

Find all `.log` files:

``` bash
find . -type f -name "*.log"
```

Search for errors in every `.log` file and save matching lines:

``` bash
grep -h "ERROR" $(find . -type f -name "*.log") > reports/all-errors.log
```

Count the saved error lines:

``` bash
wc -l reports/all-errors.log
```

A safer approach, especially when filenames contain spaces, is:

``` bash
find . -type f -name "*.log" -exec grep -h "ERROR" {} + > reports/all-errors.log
```

Find unique error messages across all log files:

``` bash
find . -type f -name "*.log" -exec grep "ERROR" {} + \
  | sed 's/^.*ERROR //' \
  | sort -u
```

Find backup archives:

``` bash
find backups -type f -name "*.tar"
```

Inspect configuration permissions:

``` bash
ls -l config
```

Verify the temporary file is gone:

``` bash
find reports -name "temp.txt"
```

If no result is printed, the file was not found.

Inspect the final project structure:

``` bash
find . -print | sort
```

> **Note about `grep -h`:** It suppresses filenames in the output. The
> `find ... -exec ... {} +` pattern is generally safer than putting
> `$(find ...)` into a command because it handles filenames with spaces
> more reliably.

## 10. Command Reference

  Command         Main purpose
  --------------- ----------------------------------------------------
  `pwd`           Print current directory
  `ls`            List files and directories
  `cd`            Change directory
  `mkdir`         Create directories
  `touch`         Create an empty file / update timestamp
  `cp`            Copy files
  `mv`            Move or rename files
  `rm`            Remove files
  `cat`           Print file contents
  `less`          Read a file interactively
  `head`          Display the beginning of a file
  `tail`          Display the end of a file
  `sudo`          Run an authorized command with elevated privileges
  `apt update`    Refresh package indexes
  `apt search`    Search available packages
  `apt install`   Install a package
  `nano`          Edit text in the terminal
  `whoami`        Display effective username
  `id`            Display UID, GID, and groups
  `groups`        Display group memberships
  `ls -l`         Show detailed file information and permissions
  `chmod`         Change permissions
  `chown`         Change ownership
  `grep`          Search text
  `find`          Find files and directories
  `sort`          Sort lines
  `uniq`          Filter adjacent duplicate lines
  `wc`            Count lines, words, or bytes
  `\|`            Pipe output into another command
  `>`             Redirect output and overwrite a file
  `>>`            Redirect output and append to a file

## 11. GitHub Submission Structure

Document your work in your `devops-learning` repository:

``` text
devops-learning/
└── linux/
    └── master-challenge/
        ├── README.md
        ├── commands.md
        ├── findings.md
        └── screenshots/
```

Suggested contents:

-   `README.md`: challenge objective, environment, phases, and final
    result.
-   `commands.md`: commands you used and what each one does.
-   `findings.md`: errors found, frequency results, permission changes,
    and issues you solved.
-   `screenshots/`: terminal evidence, with no passwords, tokens, or
    other secrets visible.

## 12. Self-Assessment

Use this checklist to assess whether you can perform the tasks without
copying a solution:

-   [ ] Create and inspect a nested directory structure.
-   [ ] Navigate using both absolute and relative paths.
-   [ ] Create, copy, move, rename, and remove lab files.
-   [ ] Read files using `cat` and `less`.
-   [ ] Display the first and last five lines of a log.
-   [ ] Search logs using `grep`.
-   [ ] Count lines using `wc`.
-   [ ] Use `>` and `>>` correctly.
-   [ ] Explain why `sort` is needed before `uniq`.
-   [ ] Generate unique-error and error-frequency reports.
-   [ ] Explain `rwx` permissions and apply `chmod`.
-   [ ] Inspect user identity and group membership.
-   [ ] Explain when `sudo` is needed.
-   [ ] Search for and install a package using APT.
-   [ ] Find files using `find`.
-   [ ] Combine commands using pipes and redirection.

------------------------------------------------------------------------

**Completion standard:** You should be able to explain what each command
does, why you chose it, what output you expected, and how you would
troubleshoot an unexpected result. The goal is practical
understanding---not memorizing command sequences.
