# Linux Processes

## 1. What is a Process?

A **process** is a running instance of a program.

Examples:
- Nginx running = Process
- MongoDB running = Process
- Node.js server running = Process
- Docker daemon running = Process

**Important:** A program is a file, while a process is its running instance.

## 2. What is PID?

**PID (Process ID)** is a unique ID assigned to every running process by Linux.

```bash
ps aux
```
Shows all running processes.

```bash
ps aux | grep node
```
Searches for Node-related processes.

## 3. Important Process Commands

| Command | Purpose |
|---|---|
| `ps aux` | Show all processes |
| `top` | Monitor processes live |
| `htop` | Interactive process monitoring |
| `pgrep node` | Find Node process PID |
| `pgrep -a node` | Show PID and full command |
| `kill PID` | Stop a process |
| `kill -9 PID` | Forcefully stop a process |
| `jobs` | Show background jobs |
| `nice` | Set process priority |
| `renice` | Change running process priority |

## 4. Foreground vs Background

### Foreground Process

A process that occupies the terminal while running.

```bash
ping google.com
```

Stop it using:

```bash
Ctrl + C
```

### Background Process

A process that runs in the background, allowing you to use the terminal.

```bash
ping google.com &
```

Example:

```bash
sleep 300 &
jobs
```

- `&` runs a command in the background.
- `jobs` shows background jobs.

## 5. How to Stop a Process?

Find the process:

```bash
pgrep -a node
```

Stop normally:

```bash
kill 1234
```

Forcefully stop:

```bash
kill -9 1234
```

**Important:**

- `kill` sends SIGTERM (15) by default. It gives the process a chance to shut down gracefully.
- `kill -9` sends SIGKILL. It immediately terminates the process without allowing cleanup.

Always try normal `kill` first. Use `kill -9` as a last option.

## 6. Process Priority (Nice Value)

Linux assigns priority to processes. Nice Value controls how much CPU scheduling preference a process receives.

Range: `-20` to `19`

- `-20` = Highest priority
- `19` = Lowest priority
- `0` = Default priority

Start a process with lower priority:

```bash
nice -n 10 python app.py
```

Change priority of a running process:

```bash
sudo renice 5 -p 1234
```

## 7. grep vs pgrep

Both commands search, but their purposes are different.

| grep | pgrep |
|---|---|
| Searches text | Searches running processes |
| Works with files and command output | Works with process names |
| General-purpose search | Process-specific search |
| Shows matching text | Shows matching PIDs |

**Examples:**

Search Node processes in command output:

```bash
ps aux | grep node
```

Find Node process PID directly:

```bash
pgrep node
```

Show PID and complete command:

```bash
pgrep -a node
```

**Remember:** Use `grep` for general text searching and `pgrep` for process management.

## 8. Practical Lab

Create a background process:

```bash
sleep 300 &
```

Check background jobs:

```bash
jobs
```

Find its PID:

```bash
pgrep -a sleep
```

Check process details:

```bash
ps aux | grep sleep
```

Stop the process:

```bash
kill PID
```

Confirm it stopped:

```bash
pgrep -a sleep
```

---

**Next Topics:**
- PPID (Parent Process ID)
- `pstree`
- Process States (R, S, Z)
- Zombie Processes
